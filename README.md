using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("=== Calculadora de Préstamos ===\n");

        double monto = PedirDatoPositivo("Monto del préstamo: ");
        double tasaAnual = PedirDatoPositivo("Tasa de interés anual (%): ");
        double anios = PedirDatoPositivo("Número de años: ");

        double pagoMensual = CalcularPagoMensual(monto, tasaAnual, anios);
        double pagoTotal = CalcularPagoTotal(pagoMensual, anios);

        Console.WriteLine("\n=== Resultados ===");
        Console.WriteLine($"Monto del préstamo:   {monto:C}");
        Console.WriteLine($"Tasa de interés anual: {tasaAnual}%");
        Console.WriteLine($"Plazo:                {anios} año(s)");
        Console.WriteLine($"Pago mensual:         {pagoMensual:C}");
        Console.WriteLine($"Total a pagar:        {pagoTotal:C}");
        Console.WriteLine($"Intereses totales:    {(pagoTotal - monto):C}");
    }

    // Pide un dato al usuario hasta que sea un número positivo válido.
    static double PedirDatoPositivo(string mensaje)
    {
        while (true)
        {
            Console.Write(mensaje);
            string entrada = Console.ReadLine();

            try
            {
                double valor = double.Parse(entrada);

                if (valor <= 0)
                {
                    Console.WriteLine("Error: el valor debe ser mayor que cero. Intenta de nuevo.\n");
                    continue;
                }

                return valor;
            }
            catch (FormatException)
            {
                Console.WriteLine("Error: debes ingresar un número válido (por ejemplo, 15000 o 7.5). Intenta de nuevo.\n");
            }
            catch (ArgumentNullException)
            {
                Console.WriteLine("Error: no ingresaste ningún valor. Intenta de nuevo.\n");
            }
            catch (OverflowException)
            {
                Console.WriteLine("Error: el número es demasiado grande. Intenta de nuevo.\n");
            }
        }
    }

    // Devuelve el pago mensual con la fórmula de amortización estándar.
    static double CalcularPagoMensual(double monto, double tasaAnual, double anios)
    {
        double tasaMensual = tasaAnual / 100 / 12;
        double numeroPagos = anios * 12;

        double factor = Math.Pow(1 + tasaMensual, numeroPagos);
        return monto * (tasaMensual * factor) / (factor - 1);
    }

    // Devuelve el monto total pagado durante todo el préstamo.
    static double CalcularPagoTotal(double pagoMensual, double anios)
    {
        return pagoMensual * anios * 12;
    }
}
