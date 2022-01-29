using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
//copyrights Marcin Ćwiek 2021 - nr albumu 17861
//korzystałem z: Wstęp do programowania w C#. Anna Kempa, Tomasz Staś
//docs.microsoft.com

namespace Project1
{
    class ProsteMenu //klasa statyczna ProsteMenu

    {
        static Waga waga = new Waga();


        public void StartProsteMenu()
        {

            ConsoleKeyInfo klawisz;
            Console.Title = "Proste menu";// instrukcja przypisania tytułu do Menu

            do 
            {
                Console.Clear();// czyścimy konsole
                Console.WriteLine(">>>>Przygotował Marcin Ćwiek, 17861, All rights reserved! <<<");
                Console.WriteLine("_______________________________");
                Console.WriteLine("Notatnik treningowy");
                Console.WriteLine(">>>>Menu wyboru<<<");// nagłówki i opcje
                Console.WriteLine("1 - Waga początkowa");
                Console.WriteLine("2 - Waga docelowa");
                Console.WriteLine("3 - Ilość dni treningu w tygodniu");
                Console.WriteLine("4 - Wyświetl wprowadzone dane");
                Console.WriteLine("5 - Zapisz do pliku");
                Console.WriteLine("6 - Odczytaj swoje dane z Pliku");
                Console.WriteLine("7 - Zakończ działanie programu");
                Console.WriteLine("______________________________");


                klawisz = Console.ReadKey();//wyczekanie na wcisniecie klawisza
                switch (klawisz.Key)// Console Key Enumeration D1, D2, D3 - cyfra 1, 2, 3 instrukcja wyboru menu. Switch case: instrukcja wyboru.
                {
                    case ConsoleKey.D1:
                        Console.Clear(); Pytanie1(); break;// nacisniecie klawisza 1 wywoła opcje 1 z Menu

                    case ConsoleKey.D2:
                        Console.Clear(); Pytanie2(); break;// jw, ale klawisz 1 wywoła opcje 2

                    case ConsoleKey.D3:
                        Console.Clear(); Pytanie3(); break;// analogicznie według menu

                    case ConsoleKey.D4:
                        Console.Clear(); WyświetlWszystkieDane(); break;// program wyświetli wprowadzone przez usera dane
                    case ConsoleKey.D5:
                        Console.Clear(); ZapiszDoPliku(); break;// program zapisze dane do pliku .txt
                    case ConsoleKey.D6:
                        Console.Clear(); WczytajZPliku(); break; //program wyświetli zapisane dane z pliku .txt
                    case ConsoleKey.D7: // opcja 6 z menu, zakonczy program
                    case ConsoleKey.Escape: // klawisz esc zakonczy program

                        Environment.Exit(0); break;// metoda environment powoduje zakonczenie programu
                    default: break;
                }
            } while (klawisz.Key != ConsoleKey.D7);
        }
        static void Pytanie1()// metoda statyczna Pytanie1
        {

            Console.WriteLine("Podaj ile ważysz w kg");

            try // instrukcja try...catch 
                //try określa procedury obsługi dla róznych wyjątków (catch)
            {
                waga.wagaPoczatkowa = Int32.Parse(Console.ReadLine());// wpisanie wagi do pierwszej tablicy
            }
            catch (FormatException)// wyjątkiem jest wpisanie liczb innych niż całkowite
            {
                Console.WriteLine("Waga musi być podana w liczbach całkowitych !");// komunikat jesli user wybierze znak inny niz liczby calkowite

            }
            Console.WriteLine("Wciśnij ESC aby powrócić do Menu, odpowiedz na kolejne pytanie...");
            Console.ReadKey();
        }


        static void Pytanie2()
        {


            Console.WriteLine("Podaj ile chcesz ważyć w kg i naciśnij ENTER");
            try
            {
                waga.wagaKoncowa = Int32.Parse(Console.ReadLine());

                if(waga.wagaKoncowa >= waga.wagaPoczatkowa)//instrukcja if...else, program sprawdzi poprawność wpisanych wartości i zadziała w dwie strony, albo upomni usera, jak poniżej, albo poda poprawny cel pseudo kalkulacji
                {
                    Console.WriteLine("Wprowadzona waga nie może być większa niż " + waga.wagaPoczatkowa + " kg");// jeśli user wprowadzi wartość (kg) większą niż waga startowa, to program wyświetli komunikat zwrotny, nie obliczy dalej
                    Console.WriteLine("Pamiętaj że chcesz schudnąć");

                    waga.wagaKoncowa = 0;
                }
                else
                {
                    Console.WriteLine("Oto Twój cel : " + waga.wagaKoncowa + " kg."); //program wyświetli poprawną wagę oczekiwaną
                    
                }
            }
            catch (FormatException)
            {
                Console.WriteLine("Waga musi być podana w liczbach !");// komunikat gdy user uzyje znaku innego niz liczbowy

            }
            

            Console.ReadKey();
        }

        static void Pytanie3()
        {

            Console.WriteLine("Ilość dni treningu w tygodniu? Po wpisaniu naciśnij ENTER");// program potrzebuje liczby dni treningu, by przeliczyć postęp


            try// znów blok catch z wyjątkiem try
            {
                waga.iloscDniTreningu = Int32.Parse(Console.ReadLine());

            }
            catch (FormatException)
            {
                Console.WriteLine("Dni muszą być wartością liczbowa od 1 do 7"); // dni treningowe w skali jednego tygodnia

            }
            if (waga.iloscDniTreningu > 0 && waga.iloscDniTreningu <= 7) 
            {
                Console.WriteLine("Ilość dni treningu w tygodniu to : " + waga.iloscDniTreningu);// Konsola wyświetli ilość dni które podał user, dla potwierdzenia
                Console.ReadKey();

            }
            else
            {
                Console.WriteLine("Liczba dni treningu jest nieprawidłowa!!!");// jeśli wartość będzie większa niż 7 - wyjątek po ktorym program nie wykona dalszych operacji
                waga.iloscDniTreningu = 0;
                Console.ReadKey();
            }


        }

        static void WyświetlWszystkieDane() //funkcja static void która wyświetli userowi dane
        {
            Console.Clear();
            Console.WriteLine("Wprowadziłeś następujące dane do systemu : "); //wyswietlenie wprowadzonych przez usera danych
            Console.WriteLine("Waga Początkowa : " + waga.wagaPoczatkowa + " kg");
            Console.WriteLine("Waga Docelowa : " + waga.wagaKoncowa + " Kg");
            Console.WriteLine("Ilość dni treningu w tygodniu : " + waga.iloscDniTreningu );
            Console.WriteLine();
            ObliczanieSpadkuWagi();

            Console.ReadKey();
        }
        static void ZapiszDoPliku() //zapis do pliku
        {

            try
            {
                
                StreamWriter sw = new StreamWriter("D:\\Waga.txt"); // plik
                
                sw.WriteLine(waga.wagaPoczatkowa);  //1 wartosc zapisana w pliku
                
                sw.WriteLine(waga.wagaKoncowa); //2 wartosc zapisana w pliku
                
                sw.Write(waga.iloscDniTreningu); // 3 wartosc zapisana
                sw.Close();
                Console.Clear();
                Console.Beep();
                Console.WriteLine("Dane zostały zapisane do pliku");
                Console.ReadKey();
            }
            catch (Exception e)// wyjątek
            {
                Console.WriteLine("Exception: " + e.Message);
            }
            finally
            {
                Console.WriteLine("Executing finally block.");
            }

        }

        static void WczytajZPliku()
        {
            String line;
            try
            {
                StreamReader sr = new StreamReader("D:\\Waga.txt");// StreamReader z klasy System.IO
                //StreamReader odczytuje tekst z pliku 

                Console.WriteLine("Z Pliku zostały wczytane następujące dane :"); 

                for (int a = 0; a < 3; a++) { // w poniższych trzeb przypadkach używam pętli for, by wczytać podaną wagę początkową, a == 0
                   
                    if (a == 0)
                    {
                        line = sr.ReadLine();
                        waga.wagaPoczatkowa = int.Parse(line);
                        Console.WriteLine("Waga Początkowa to : " + waga.wagaPoczatkowa + " Kg");
                        
                    }
                    if (a == 1) //wczytamy wagę końcową  a<3
                    {
                        
                        line = sr.ReadLine();
                        waga.wagaKoncowa = int.Parse(line);
                        Console.WriteLine("Waga Końcowa to : " + waga.wagaKoncowa + " Kg");
                        
                        
                    }
                    if (a == 2) //wczytujemy ilość dni treningowych a<3
                    {
                        line = sr.ReadLine();
                        waga.iloscDniTreningu = int.Parse(line);
                        Console.WriteLine("Ilość dni treningu w tygodniu : " + waga.iloscDniTreningu);
                       
                    }
                }
                Console.Beep();//odtworzy drzwięk/sygnał
                sr.Close();
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception: " + e.Message);
            }
            finally
            {
                Console.WriteLine("Executing finally block.");
            }
        }

        static void ObliczanieSpadkuWagi()//Najważniejsza formuła, wynik tego progamu. W tym notatniku, a zarazem pseudokalkulatorze spadku wagi
        {
            double wagaStartowa = waga.wagaPoczatkowa; // waga startowa
            double spadekWaginaNaDzien = 0.10;
            int lp = 0;
            
            Console.WriteLine("Obliczanie spadku wagi dla podanych danych");// wyświetla userowi

            do
            { 
                for (int a = 0; a < waga.iloscDniTreningu; a++)
                {
                    wagaStartowa = wagaStartowa - spadekWaginaNaDzien;// jak to zostało obliczone, pętla do while
                }
                wagaStartowa = Math.Round(wagaStartowa, 2);
                Console.WriteLine((lp + 1) + " Tydzień treningu : " + wagaStartowa  +" Kg");//waga startowa, 1 tydzień jako 1 okres spadku wagi
                lp++;

            } while (lp != 5);
            Console.WriteLine("Osiągniesz wymażoną wage za " + ObliczanieTygodni() +" tygodni");
        }
        static int ObliczanieTygodni()// jak obliczono tygodnie
        {
            double wagaStartowa = waga.wagaPoczatkowa;
            double wagaKoncowa = waga.wagaKoncowa;
            double spadekWaginaNaDzien = 0.10;// 0.10 tyle w zamyśle powinien wynieść spadek wagi na dzien, nalezy pamietac ze jest to pseudokalkulator
            int lp = -1;

            while (wagaStartowa >= wagaKoncowa)//pętla while, powtórzy kod tak długo aż warunek zostanie spełniony, czyli uzyskamy wagę końcową
            {
                ++lp;

                for (int a = 0; a < waga.iloscDniTreningu; a++)// waga na dzień i ilość dni treningu
                {
                    wagaStartowa = wagaStartowa - spadekWaginaNaDzien;// spadek wagi na dzien 0,10 *3 dni treningowe daje nam 0,3kg w cyklu tygodniowym
                }

            } 

            return lp;
        }
    }
    -----------------------------------------------------------------------------------------------------------
    using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Project1
{
    class Program
    {
        static void Main(string[] args)
        {
            ProsteMenu proste = new ProsteMenu();
            proste.StartProsteMenu();
        }
       

    }
}
------------------------------------------------------------------------------------------------------------
using System;
using System.Collections.Generic;
using System.Text;

namespace Project1
{
    class Waga// klasa utworzona dla pseudokalkulacji spadku wagi
    {
        public int wagaPoczatkowa { get; set; }// tak jak opisane w ProsteMenu etc
        public int wagaKoncowa { get; set; }
        public int iloscDniTreningu { get; set; }
    }
}
------------------------------------------------------------------------------------------------------------

}
       
