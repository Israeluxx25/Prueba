import java.text.DecimalFormat;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class MatricesCalculator {
    private static Scanner scanner = new Scanner(System.in);
    private static Map<String, double[][]> matrices = new HashMap<>();

    // Switch que define las opciones del menú
    public static void main(String[] args) {
        int opcion;
        do {
            mostrarMenu();
            opcion = obtenerEntero("Ingrese una opción: ");
            switch (opcion) {
                case 1:
                    definirMatriz();
                    break;
                case 2:
                    sumarMatrices();
                    break;
                case 3:
                    restarMatrices();
                    break;
                case 4:
                    multiplicarMatrices();
                    break;
                case 5:
                    multiplicarPorEscalar();
                    break;
                case 6:
                    calcularDeterminante();
                    break;
                case 7:
                    imprimirMatriz();
                    break;
                case 8:
                    System.out.println("Gracias por usar la calculadora de matrices. ¡Hasta luego!");
                    break;
                default:
                    System.out.println("\u001B[31mOpción inválida. Ingrese un número válido.\u001B[0m");
            }
        } while (opcion != 8);
    }
        
        //Menú principal
    public static void mostrarMenu() {
        System.out.println("\u001B[34m--- Calculadora de Matrices ---\u001B[0m");
        System.out.println("1. Definir una matriz");
        System.out.println("2. Sumar matrices");
        System.out.println("3. Restar matrices");
        System.out.println("4. Multiplicar matrices");
        System.out.println("5. Multiplicar matriz por escalar");
        System.out.println("6. Calcular determinante");
        System.out.println("7. Imprimir una matriz");
        System.out.println("8. Salir");
    }

    // Definidor de matriz
    public static void definirMatriz() {
        System.out.println("Definir una matriz");
        String nombreMatriz = obtenerNombreMatriz("Ingrese el nombre de la matriz: ");
        int filas = obtenerEntero("Ingrese el número de filas: ");
        int columnas = obtenerEntero("Ingrese el número de columnas: ");
        double[][] matriz = new double[filas][columnas];
        System.out.println("Ingrese los elementos de la matriz:");
        for (int i = 0; i < filas; i++) {
            for (int j = 0; j < columnas; j++) {
                matriz[i][j] = obtenerDecimal("Ingrese el elemento en la posición (" + (i + 1) + "," + (j + 1) + "): ");
            }
        }
        matrices.put(nombreMatriz, matriz);
        System.out.println("La matriz \"" + nombreMatriz + "\" ha sido definida correctamente.");
    }

    // Suma de matrices
    public static void sumarMatrices() {
        System.out.println("Sumar matrices");
        String nombreMatrizA = obtenerNombreMatriz("Ingrese el nombre de la primera matriz: ");
        String nombreMatrizB = obtenerNombreMatriz("Ingrese el nombre de la segunda matriz: ");
        if (matrices.containsKey(nombreMatrizA) && matrices.containsKey(nombreMatrizB)) {
            double[][] matrizA = matrices.get(nombreMatrizA);
            double[][] matrizB = matrices.get(nombreMatrizB);
            if (dimensionesValidas(matrizA, matrizB)) {
                double[][] resultado = sumarMatrices(matrizA, matrizB);
                mostrarResultado(resultado);
            } else {
                System.out.println("\u001B[31mError: Las matrices no tienen dimensiones compatibles para la suma.\u001B[0m");
            }
        } else {
            System.out.println("\u001B[31mEl nombre de la matriz no es válido.\u001B[0m");
        }
    }

        // Resta de matrices
    public static void restarMatrices() {
        System.out.println("Restar matrices");
        String nombreMatrizA = obtenerNombreMatriz("Ingrese el nombre de la primera matriz: ");
        String nombreMatrizB = obtenerNombreMatriz("Ingrese el nombre de la segunda matriz: ");
        if (matrices.containsKey(nombreMatrizA) && matrices.containsKey(nombreMatrizB)) {
            double[][] matrizA = matrices.get(nombreMatrizA);
            double[][] matrizB = matrices.get(nombreMatrizB);
            if (dimensionesValidas(matrizA, matrizB)) {
                double[][] resultado = restarMatrices(matrizA, matrizB);
                mostrarResultado(resultado);
            } else {
                System.out.println("\u001B[31mError: Las matrices no tienen dimensiones compatibles para la resta.\u001B[0m");
            }
        } else {
            System.out.println("\u001B[31mEl nombre de la matriz no es válido.\u001B[0m");
        }
    }

        // Multiplicación de matrices
    public static void multiplicarMatrices() {
        System.out.println("Multiplicar matrices");
        String nombreMatrizA = obtenerNombreMatriz("Ingrese el nombre de la primera matriz: ");
        String nombreMatrizB = obtenerNombreMatriz("Ingrese el nombre de la segunda matriz: ");
        if (matrices.containsKey(nombreMatrizA) && matrices.containsKey(nombreMatrizB)) {
            double[][] matrizA = matrices.get(nombreMatrizA);
            double[][] matrizB = matrices.get(nombreMatrizB);
            if (dimensionesValidasParaMultiplicacion(matrizA, matrizB)) {
                double[][] resultado = multiplicarMatrices(matrizA, matrizB);
                mostrarResultado(resultado);
            } else {
                System.out.println("\u001B[31mError: Las matrices no tienen dimensiones compatibles para la multiplicación.\u001B[0m");
            }
        } else {
            System.out.println("\u001B[31mEl nombre de la matriz no es válido.\u001B[0m");
        }
    }

        //Multiplicación de matrices por un escalar
    public static void multiplicarPorEscalar() {
        System.out.println("Multiplicar matriz por escalar");
        String nombreMatriz = obtenerNombreMatriz("Ingrese el nombre de la matriz: ");
        if (matrices.containsKey(nombreMatriz)) {
            double[][] matriz = matrices.get(nombreMatriz);
            double escalar = obtenerDecimal("Ingrese el escalar: ");
            double[][] resultado = multiplicarPorEscalar(matriz, escalar);
            mostrarResultado(resultado);
        } else {
            System.out.println("\u001B[31mEl nombre de la matriz no es válido.\u001B[0m");
        }
    }

        // Calcula el determinante 
    public static void calcularDeterminante() {
        System.out.println("Calcular determinante");
        String nombreMatriz = obtenerNombreMatriz("Ingrese el nombre de la matriz: ");
        if (matrices.containsKey(nombreMatriz)) {
            double[][] matriz = matrices.get(nombreMatriz);
            if (esMatrizCuadrada(matriz)) {
                double determinante = calcularDeterminante(matriz);
                System.out.println("El determinante de la matriz \"" + nombreMatriz + "\" es: " + determinante);
            } else {
                System.out.println("\u001B[31mError: La matriz debe ser cuadrada para calcular el determinante.\u001B[0m");
            }
        } else {
            System.out.println("\u001B[31mEl nombre de la matriz no es válido.\u001B[0m");
        }
    }

        // Imprime la matriz 
    public static void imprimirMatriz() {
        System.out.println("Imprimir una matriz");
        String nombreMatriz = obtenerNombreMatriz("Ingrese el nombre de la matriz: ");
        if (matrices.containsKey(nombreMatriz)) {
            double[][] matriz = matrices.get(nombreMatriz);
            System.out.println("La matriz \"" + nombreMatriz + "\" es:");
            for (int i = 0; i < matriz.length; i++) {
                for (int j = 0; j < matriz[0].length; j++) {
                    System.out.print(matriz[i][j] + " ");
                }
                System.out.println();
            }
        } else {
            System.out.println("\u001B[31mEl nombre de la matriz no es válido.\u001B[0m");
        }
    }

    public static String obtenerNombreMatriz(String mensaje) {
        System.out.print(mensaje);
        return scanner.nextLine();
    }

        // Lee los numeros enteros 
    public static int obtenerEntero(String mensaje) {
        while (true) {
            try {
                System.out.print(mensaje);
                return Integer.parseInt(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.println("\u001B[31mError: Ingrese un número entero válido.\u001B[0m");
            }
        }
    }

    // Lee decimales
    public static double obtenerDecimal(String mensaje) {
        while (true) {
            try {
                System.out.print(mensaje);
                return Double.parseDouble(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.println("\u001B[31mError: Ingrese un número decimal válido.\u001B[0m");
            }
        }
    }

        // Boleano que lee las filas y columnas 
    public static boolean dimensionesValidas(double[][] matrizA, double[][] matrizB) {
        return matrizA.length == matrizB.length && matrizA[0].length == matrizB[0].length;
    }

    public static boolean dimensionesValidasParaMultiplicacion(double[][] matrizA, double[][] matrizB) {
        return matrizA[0].length == matrizB.length;
    }

    public static boolean esMatrizCuadrada(double[][] matriz) {
        return matriz.length == matriz[0].length;
    }

            // Parte que hace la suma 
    public static double[][] sumarMatrices(double[][] matrizA, double[][] matrizB) {
        int filas = matrizA.length;
        int columnas = matrizA[0].length;
        double[][] resultado = new double[filas][columnas];
        for (int i = 0; i < filas; i++) {
            for (int j = 0; j < columnas; j++) {
                resultado[i][j] = matrizA[i][j] + matrizB[i][j];
            }
        }
        return resultado;
    }

            // Parte que hace la resta 
    public static double[][] restarMatrices(double[][] matrizA, double[][] matrizB) {
        int filas = matrizA.length;
        int columnas = matrizA[0].length;
        double[][] resultado = new double[filas][columnas];
        for (int i = 0; i < filas; i++) {
            for (int j = 0; j < columnas; j++) {
                resultado[i][j] = matrizA[i][j] - matrizB[i][j];
            }
        }
        return resultado;
    }

            // parte que hace las multiplicaciones 
    public static double[][] multiplicarMatrices(double[][] matrizA, double[][] matrizB) {
        int filasA = matrizA.length;
        int columnasA = matrizA[0].length;
        int columnasB = matrizB[0].length;
        double[][] resultado = new double[filasA][columnasB];
        for (int i = 0; i < filasA; i++) {
            for (int j = 0; j < columnasB; j++) {
                for (int k = 0; k < columnasA; k++) {
                    resultado[i][j] += matrizA[i][k] * matrizB[k][j];
                }
            }
        }
        return resultado;
    }

            // Parte que hace la multiplicación por un escalar
    public static double[][] multiplicarPorEscalar(double[][] matriz, double escalar) {
        int filas = matriz.length;
        int columnas = matriz[0].length;
        double[][] resultado = new double[filas][columnas];
        for (int i = 0; i < filas; i++) {
            for (int j = 0; j < columnas; j++) {
                resultado[i][j] = matriz[i][j] * escalar;
            }
        }
        return resultado;
    }

            //parte que calcula determinantes 
    public static double calcularDeterminante(double[][] matriz) {
        int n = matriz.length;
        if (n == 1) {
            return matriz[0][0];
        } else if (n == 2) {
            return matriz[0][0] * matriz[1][1] - matriz[0][1] * matriz[1][0];
        } else {
            double determinante = 0;
            for (int j = 0; j < n; j++) {
                determinante += Math.pow(-1, j) * matriz[0][j] * calcularDeterminante(obtenerSubmatriz(matriz, 0, j));
            }
            return determinante;
        }
    }

        // Parte que lee la cantidad de columnas y filas 
    public static double[][] obtenerSubmatriz(double[][] matriz, int fila, int columna) {
        int n = matriz.length;
        double[][] submatriz = new double[n - 1][n - 1];
        int contadorFila = 0;
        int contadorColumna;
        for (int i = 0; i < n; i++) {
            if (i != fila) {
                contadorColumna = 0;
                for (int j = 0; j < n; j++) {
                    if (j != columna) {
                        submatriz[contadorFila][contadorColumna] = matriz[i][j];
                        contadorColumna++;
                    }
                }
                contadorFila++;
            }
        }
        return submatriz;
    }
        //Esto presenta el resultado 
    public static void mostrarResultado(double[][] resultado) {
        DecimalFormat formato = new DecimalFormat("#.00");
        System.out.println("El resultado es:");
        for (int i = 0; i < resultado.length; i++) {
            for (int j = 0; j < resultado[0].length; j++) {
                System.out.print(formato.format(resultado[i][j]) + " ");
            }
            System.out.println();
        }
    }
}
