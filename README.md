# calculadora
from flask import Flask, render_template, request, jsonify

app = Flask(__name__)

def calcular(num1, num2, operacion):
    """Realiza la operación matemática según el tipo especificado."""
    try:
        num1, num2 = float(num1), float(num2)
        if operacion == "sum":
            return num1 + num2
        elif operacion == "subtract":
            return num1 - num2
        elif operacion == "multiply":
            return num1 * num2
        elif operacion == "divide":
            return num1 / num2 if num2 != 0 else "Error: División por cero"
        else:
            return "Error: Operación no válida"
    except ValueError:
        return "Error: Entrada no válida"

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/api/calculadora", methods=["POST"])
def api_calculadora():
    data = request.get_json()
    num1 = data.get("num1")
    num2 = data.get("num2")
    operation = data.get("operation")
    
    result = calcular(num1, num2, operation)
    return jsonify({"result": result})

if __name__ == "__main__":
    app.run(debug=True)
