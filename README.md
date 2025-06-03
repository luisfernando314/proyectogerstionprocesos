#include <iostream>
#include <string>
using namespace std;

int main() {
    const int MAX = 100;

    // Arreglos para los datos
    string nombre[MAX];
    float precioCompra[MAX], ganancia[MAX], precioVenta[MAX];
    char categoria[MAX];
    int n = 0; // Cantidad real de productos registrados
    bool datosIngresados = false; // Bandera para saber si se ingresaron productos

    int opcion;

    do {
        // MENÃš DE OPCIONES
        cout << "\n=== MENU DE OPCIONES ===\n";
        cout << "1. Registrar productos\n";
        cout << "2. Reporte 1: Lista de productos\n";
        cout << "3. Reporte 2: Productos de categorÃ­a B o C\n";
        cout << "4. Reporte 3: Productos de categorÃ­a C con precio venta > 100\n";
        cout << "0. Salir\n";
        cout << "Seleccione una opciÃ³n: ";
        cin >> opcion;

        switch (opcion) {
            case 1: {
                do {
                    cout << "Ingrese la cantidad de productos (1 - 100): ";
                    cin >> n;
                } while (n < 1 || n > 100);

                for (int i = 0; i < n; i++) {
                    cin.ignore(); // limpiar buffer
                    cout << "\nProducto #" << (i + 1) << endl;

                    cout << "Nombre: ";
                    getline(cin, nombre[i]);

                    do {
                        cout << "Precio de compra (> 0): ";
                        cin >> precioCompra[i];
                    } while (precioCompra[i] <= 0);

                    do {
                        cout << "CategorÃ­a (A/B/C): ";
                        cin >> categoria[i];
                        categoria[i] = toupper(categoria[i]);
                    } while (categoria[i] != 'A' && categoria[i] != 'B' && categoria[i] != 'C');

                    float porcentaje = 0;
                    if (categoria[i] == 'A') porcentaje = 0.15;
                    else if (categoria[i] == 'B') porcentaje = 0.25;
                    else porcentaje = 0.35;

                    ganancia[i] = precioCompra[i] * porcentaje;
                    precioVenta[i] = precioCompra[i] + ganancia[i];
                }

                datosIngresados = true;
                break;
            }

            case 2: {
                if (!datosIngresados) {
                    cout << "Primero debe registrar productos.\n";
                    break;
                }

                cout << "\n--- REPORTE 1: Lista de productos ---\n";
                float totalVenta = 0;

                for (int i = 0; i < n; i++) {
                    cout << nombre[i] << " - "
                         << (int)(precioCompra[i] * 100 + 0.5) / 100.0 << " - "
                         << (int)(ganancia[i] * 100 + 0.5) / 100.0 << " - "
                         << (int)(precioVenta[i] * 100 + 0.5) / 100.0 << endl;

                    totalVenta += precioVenta[i];
                }

                cout << "Total general de precios de venta: "
                     << (int)(totalVenta * 100 + 0.5) / 100.0 << endl;
                break;
            }

            case 3: {
                if (!datosIngresados) {
                    cout << "Primero debe registrar productos.\n";
                    break;
                }

                cout << "\n--- REPORTE 2: Productos de categorÃ­a B o C ---\n";
                int contadorB = 0, contadorC = 0;

                for (int i = 0; i < n; i++) {
                    if (categoria[i] == 'B' || categoria[i] == 'C') {
                        cout << nombre[i] << " - " << categoria[i] << endl;
                        if (categoria[i] == 'B') contadorB++;
                        else contadorC++;
                    }
                }

                cout << "Cantidad categorÃ­a B: " << contadorB << endl;
                cout << "Cantidad categorÃ­a C: " << contadorC << endl;
                break;
            }

            case 4: {
                if (!datosIngresados) {
                    cout << "Primero debe registrar productos.\n";
                    break;
                }

                cout << "\n--- REPORTE 3: Productos con precio venta > 100 y categorÃ­a C ---\n";
                float sumaPV = 0;
                int contadorPV = 0;

                for (int i = 0; i < n; i++) {
                    if (categoria[i] == 'C' && precioVenta[i] > 100) {
                        cout << nombre[i] << " - " << categoria[i] << " - "
                             << (int)(precioVenta[i] * 100 + 0.5) / 100.0 << endl;

                        sumaPV += precioVenta[i];
                        contadorPV++;
                    }
                }

                if (contadorPV > 0) {
                    float promedio = sumaPV / contadorPV;
                    cout << "Promedio de precios de venta: "
                         << (int)(promedio * 100 + 0.5) / 100.0 << endl;
                } else {
                    cout << "No hay productos que cumplan con la condiciÃ³n.\n";
                }
                break;
            }

            case 0:
                cout << "Saliendo del programa. Â¡Hasta luego!\n";
                break;

            default:
                cout << "OpciÃ³n no vÃ¡lida. Intente nuevamente.\n";
        }

    } while (opcion != 0);

    return 0;
}
