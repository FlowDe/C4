# C4
Connect 4


using System;

namespace VierGewinnt
{
	class VierGewinnt  
	{
		private int[,] Spielfeld = new int[7,7];
		int runde = 0;
		bool sieg;
		int zeile;
		int spalte;
		int spielernr;
		int spielerstein;

		public void vier_gewinnt(){
			runde++;

			if (runde == 1) {
				Console.WriteLine ("Zeilen und Spalten gehen jeweils von 0 bis 6!\r\n");

				for (int i = 0; i < 7; i++) {
					for (int j = 0; j < 7; j++) {
						Spielfeld [i, j] = 0;
	
						Console.Write ("{0} ",Spielfeld [i, j]);
					}
					Console.WriteLine ();
				}
				this.sieg = false;
			} 

			else {
				for (int i = 0; i < 7; i++) {
					for (int j = 0; j < 7; j++) {
						Console.Write ("{0} ", Spielfeld [i, j]);
					}
					Console.WriteLine ();
				}
			}

			if (this.sieg == false) {
				set_stone ();
			} else {
				Console.WriteLine ("\r\nSpieler {0} hat gewonnen!\r\n", this.spielernr);
			}

		}

		public void set_stone(){
			if (runde % 2 == 1) {
				this.spielernr = 1;
				this.spielerstein = 1;
			} 
			else {
				this.spielernr = 2;
				this.spielerstein = 9;
			}

			Console.Write ("\r\nSpieler {0} du bist dran. Wo setzt Du den Stein?\r\nZeile = ",this.spielernr);
			int zeile = Convert.ToInt32 (Console.ReadLine ());
			this.zeile = zeile;

			Console.Write ("Spalte = ");
			int spalte = Convert.ToInt32 (Console.ReadLine ());
			this.spalte = spalte;
			Console.WriteLine ();

			check();
		}

		public void check(){
			if(this.zeile < 0 || this.zeile > 6 || this.spalte < 0 || this.spalte > 6){
				Console.WriteLine ("Ungültige Eingabe! Nochmal!");
				set_stone ();
			}


			if (this.Spielfeld [this.zeile, this.spalte] == 0) {
				this.Spielfeld [zeile, spalte] = this.spielerstein;
				gewonnen ();
				vier_gewinnt ();
				return;
			}
			else {
				Console.WriteLine ("Feld belegt!");
				set_stone ();
			}


		}

		public void gewonnen(){
			int count = 0;
			int o = 0; int u = 0; int l = 0; int r = 0;

			switch(this.zeile){
				case 0: o = 1; u = 4;  break;
				case 1: o = 2; u = 4; break;
				case 2: o = 3; u = 4; break;
				case 6: u = 1; o = 4; break;
				case 5: u = 2; o = 4; break;
				case 4: u = 3; o = 4; break;
				default: o = 4; u = 4; break;
			}

			switch(this.spalte){
				case 0: l = 1; r = 4; break;
				case 1: l = 2; r = 4; break;
				case 2: l = 3; r = 4; break;
				case 6: r = 1; l = 4; break;
				case 5: r = 2; l = 4; break;
				case 4: r = 3; l = 4; break;
				default: l = 4; r = 4; break;
			}
				
			//nach oben
			count += zaehler (o, 0, -1, 0); //(Wie oft in zeilen richtung?, Wie oft in spalten richtung?, zeilenrichtung, spaltenrichtung)

			//nach unten
			count += zaehler (u, 0, 1, 0);

			if (count == 3) {
				this.sieg = true;
				return; // bricht Methode ab
			} else {count = 0;}


			//nach rechts
			count += zaehler (0, r, 0, 1);

			//nach links
			count += zaehler (0, l, 0, -1);

			if (count == 3) {
				this.sieg = true;
				return;
			} else {count = 0;}


			//nach rechts oben
			count += zaehler (o, r, -1, 1);

			//nach links unten
			count += zaehler (u, l, 1, -1);

			if (count == 3) {
				this.sieg = true;
				return;
			} else {count = 0;}


			//nach links oben
			count += zaehler (o, l, -1, -1);

			//nach rechts unten
			count += zaehler (u, r, 1, 1);

			if (count == 3) {
				this.sieg = true;
				return;
			} else {count = 0;}

		}

		public int zaehler(int positionz, int positions , int richtungz, int richtungs){
			int kontrollstein;
			int count = 0;
			int i = 1;
			int j = 0;
			int position = 0;
		
			if (positionz == 0 || positions == 0) {
				if (positionz == 0) {position = positions;}
				if (positions == 0) {position = positionz;}
	
				for (i = 1; i < position; i++) {
					kontrollstein = this.Spielfeld [this.zeile + i * richtungz, this.spalte + i * richtungs];
					if (kontrollstein == this.spielerstein) {
						count++;
					} else {
						break;
					}
				}
			}

			else{
				for (i = 1; i < positionz; i++) {
					if(j < (positions-1)){j++;}

					kontrollstein = this.Spielfeld [this.zeile + i * richtungz, this.spalte + j * richtungs];
					if (kontrollstein == this.spielerstein) {
						count++;
					} else {
						break;
					}
				}
			}


			return count;
		}
			
	}

	class MainClass
	{
		public static void Main (string[] args)
		{
			VierGewinnt spiel1 = new VierGewinnt ();
			spiel1.vier_gewinnt ();


		}
	}
}
