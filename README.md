
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crypto Exchange Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 20px;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
        }
        .container {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            display: inline-block;
            box-shadow: 0px 0px 10px rgba(255, 255, 255, 0.2);
        }
        select, input, button {
            margin: 10px;
            padding: 12px;
            font-size: 16px;
            border-radius: 5px;
            border: none;
        }
        select, input {
            width: 200px;
            text-align: center;
        }
        button {
            background-color: #ff9800;
            color: white;
            font-weight: bold;
            cursor: pointer;
        }
        button:hover {
            background-color: #e68900;
        }
        #result, #multiples {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="container">
        <h2>Crypto Exchange Calculator</h2>

        <label>Cryptocurrency:</label>
        <select id="crypto">
            <option value="bitcoin">Bitcoin (BTC)</option>
            <option value="ethereum">Ethereum (ETH)</option>
            <option value="dogecoin">Dogecoin (DOGE)</option>
            <option value="shiba-inu">Shiba Inu (SHIB)</option>
            <option value="solana">Solana (SOL)</option>
            <option value="cardano">Cardano (ADA)</option>
            <option value="ripple">Ripple (XRP)</option>
            <option value="polkadot">Polkadot (DOT)</option>
            <option value="litecoin">Litecoin (LTC)</option>
            <option value="tether">Tether (USDT - BEP20)</option>
        </select>

        <br>

        <label>Fiat Currency:</label>
        <select id="fiat">
            <option value="usd">US Dollar (USD)</option>
            <option value="inr">Indian Rupee (INR)</option>
            <option value="eur">Euro (EUR)</option>
            <option value="gbp">British Pound (GBP)</option>
            <option value="jpy">Japanese Yen (JPY)</option>
            <option value="aud">Australian Dollar (AUD)</option>
            <option value="cad">Canadian Dollar (CAD)</option>
        </select>

        <br>

        <label>Enter Amount:</label>
        <input type="number" id="amount" placeholder="e.g. 50" value="1">

        <br>

        <button onclick="convertCrypto()">Convert</button>

        <h3 id="result"></h3>
        <h3 id="multiples"></h3>
    </div>

    <script>
        function convertCrypto() {
            let crypto = document.getElementById("crypto").value;
            let fiat = document.getElementById("fiat").value;
            let amount = parseFloat(document.getElementById("amount").value);

            if (isNaN(amount) || amount <= 0) {
                document.getElementById("result").innerText = "Please enter a valid amount!";
                return;
            }

            let apiUrl = `https://api.coingecko.com/api/v3/simple/price?ids=${crypto}&vs_currencies=${fiat}`;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    let price = data[crypto]?.[fiat];
                    if (price) {
                        let total = price * amount;
                        document.getElementById("result").innerText = `${amount} ${crypto.toUpperCase()} = ${total.toFixed(2)} ${fiat.toUpperCase()}`;

                        let multiplesText = `Multiples:\n`;
                        for (let i = 1; i <= 10; i++) {
                            multiplesText += `${i * amount} ${crypto.toUpperCase()} = ${(i * total).toFixed(2)} ${fiat.toUpperCase()}\n`;
                        }
                        document.getElementById("multiples").innerText = multiplesText;
                    } else {
                        document.getElementById("result").innerText = "Invalid Crypto or Currency!";
                    }
                })
                .catch(error => {
                    document.getElementById("result").innerText = "Error fetching data!";
                    console.error(error);
                });
        }
    </script>

</body>
</html>
