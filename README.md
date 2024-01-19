# Main code
import sqlite3

# Conectar a la base de datos (o crearla si no existe)
conn = sqlite3.connect('modelo_datos.db')
cursor = conn.cursor()

# Verificar si la tabla "Rectangulos" existe
cursor.execute("SELECT name FROM sqlite_master WHERE type='table' AND name='Rectangulos'")
table_exists = cursor.fetchone()

# Crear la tabla solo si no existe
if not table_exists:
    cursor.execute('''
        CREATE TABLE Rectangulos (
            RectanguloID INTEGER PRIMARY KEY,
            OtrosAtributos TEXT
        )
    ''')

# Crear la tabla "Forecasts" con una clave foránea que referencia a la tabla "Rectangulos"
cursor.execute('''
    CREATE TABLE Forecasts (
        ForecastID INTEGER PRIMARY KEY,
        RectanguloID INTEGER,
        Q1 INTEGER,
        Q2 INTEGER,
        Q3 INTEGER,
        Q4 INTEGER,
        FOREIGN KEY (RectanguloID) REFERENCES Rectangulos(RectanguloID)
    )
''')

# Insertar datos de ejemplo en la tabla "Rectangulos"
cursor.execute("INSERT INTO Rectangulos (OtrosAtributos) VALUES ('Atributo1')")
cursor.execute("INSERT INTO Rectangulos (OtrosAtributos) VALUES ('Atributo2')")
cursor.execute("INSERT INTO Rectangulos (OtrosAtributos) VALUES ('Atributo3')")

# Insertar datos de ejemplo en la tabla "Forecasts"
cursor.execute("INSERT INTO Forecasts (RectanguloID, Q1, Q2, Q3, Q4) VALUES (1, 10, 15, 20, 25)")
cursor.execute("INSERT INTO Forecasts (RectanguloID, Q1, Q2, Q3, Q4) VALUES (2, 30, 35, 40, 45)")
cursor.execute("INSERT INTO Forecasts (RectanguloID, Q1, Q2, Q3, Q4) VALUES (3, 50, 55, 60, 65)")

# Guardar los cambios y cerrar la conexión
conn.commit()
conn.close()
