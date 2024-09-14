#include <iostream>
#include <string>
#include <vector>

using namespace std;

// Clase para representar una cuenta bancaria
class Cuenta {
private:
    int id;
    string nombre;
    double saldo;

public:
    Cuenta(int id, string nombre) {
        this->id = id;
        this->nombre = nombre;
        this->saldo = 0.0;
    }

    int getId() const {
        return id;
    }

    string getNombre() const {
        return nombre;
    }

    double getSaldo() const {
        return saldo;
    }

    void depositar(double monto) {
        if (monto > 0) {
            saldo += monto;
            cout << "Depósito exitoso. Nuevo saldo: $" << saldo << endl;
        } else {
            cout << "Monto no válido." << endl;
        }
    }

    void retirar(double monto) {
        if (monto > 0 && monto <= saldo) {
            saldo -= monto;
            cout << "Retiro exitoso. Nuevo saldo: $" << saldo << endl;
        } else {
            cout << "Monto no válido o saldo insuficiente." << endl;
        }
    }
};

// Prototipos de funciones
void abrirCuenta(vector<Cuenta>& cuentas);
void depositar(vector<Cuenta>& cuentas);
void retirar(vector<Cuenta>& cuentas);
void verSaldo(const vector<Cuenta>& cuentas);
int buscarCuenta(const vector<Cuenta>& cuentas, int id);

int main() {
    vector<Cuenta> cuentas;
    int opcion;

    do {
        cout << "\n*** Simulador de Banco ***" << endl;
        cout << "1. Abrir cuenta" << endl;
        cout << "2. Depositar dinero" << endl;
        cout << "3. Retirar dinero" << endl;
        cout << "4. Ver saldo" << endl;
        cout << "5. Salir" << endl;
        cout << "Seleccione una opcion: ";
        cin >> opcion;

        switch (opcion) {
            case 1:
                abrirCuenta(cuentas);
                break;
            case 2:
                depositar(cuentas);
                break;
            case 3:
                retirar(cuentas);
                break;
            case 4:
                verSaldo(cuentas);
                break;
            case 5:
                cout << "Gracias por usar el simulador de banco. ¡Adios!" << endl;
                break;
            default:
                cout << "Opción no válida, intente de nuevo." << endl;
                break;
        }
    } while (opcion != 5);

    return 0;
}

// Función para abrir una nueva cuenta
void abrirCuenta(vector<Cuenta>& cuentas) {
    int id = cuentas.size() + 1;
    string nombre;
    cout << "Ingrese el nombre del titular de la cuenta: ";
    cin.ignore();  // Limpiar el buffer
    getline(cin, nombre);

    cuentas.push_back(Cuenta(id, nombre));
    cout << "Cuenta creada con éxito. ID de cuenta: " << id << endl;
}

// Función para realizar un depósito en una cuenta
void depositar(vector<Cuenta>& cuentas) {
    int id;
    double monto;
    cout << "Ingrese el ID de la cuenta: ";
    cin >> id;

    int indice = buscarCuenta(cuentas, id);
    if (indice != -1) {
        cout << "Ingrese el monto a depositar: ";
        cin >> monto;
        cuentas[indice].depositar(monto);
    } else {
        cout << "Cuenta no encontrada." << endl;
    }
}

// Función para realizar un retiro de una cuenta
void retirar(vector<Cuenta>& cuentas) {
    int id;
    double monto;
    cout << "Ingrese el ID de la cuenta: ";
    cin >> id;

    int indice = buscarCuenta(cuentas, id);
    if (indice != -1) {
        cout << "Ingrese el monto a retirar: ";
        cin >> monto;
        cuentas[indice].retirar(monto);
    } else {
        cout << "Cuenta no encontrada." << endl;
    }
}

// Función para ver el saldo de una cuenta
void verSaldo(const vector<Cuenta>& cuentas) {
    int id;
    cout << "Ingrese el ID de la cuenta: ";
    cin >> id;

    int indice = buscarCuenta(cuentas, id);
    if (indice != -1) {
        cout << "Saldo de la cuenta de " << cuentas[indice].getNombre() << ": $" << cuentas[indice].getSaldo() << endl;
    } else {
        cout << "Cuenta no encontrada." << endl;
    }
}

// Función para buscar una cuenta por su ID
int buscarCuenta(const vector<Cuenta>& cuentas, int id) {
    for (size_t i = 0; i < cuentas.size(); i++) {
        if (cuentas[i].getId() == id) {
            return i;
        }
    }
    return -1;  // Retorna -1 si la cuenta no fue encontrada
}


