<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Print Price Calculator</title>
    <style>
        body {
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background-color: #D0C9B5;
            color: #7f423d;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 600px;
            margin: 50px auto;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            padding: 20px;
        }
        h1 {
            font-size: 2em;
            text-align: center;
            margin-bottom: 20px;
            font-weight: bold;
        }
        label {
            display: block;
            margin: 15px 0 5px;
        }
        input, select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }
        button {
            background-color: #7f423d;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1em;
        }
        button:hover {
            background-color: #5e321e;
        }
        .error {
            color: red;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 style="color: #7f423d;">NAM PRINT</h1>
        <form id="calculator-form">
            <label for="pages">จำนวนหน้า:</label>
            <input type="number" id="pages" required>

            <label for="paper-type">ความหนากระดาษ:</label>
            <select id="paper-type">
                <option value="70">70 แกรม</option>
                <option value="100">100 แกรม</option>
            </select>

            <label for="color">สี/ขาวดำ:</label>
            <select id="color">
                <option value="bw">ขาวดำ</option>
                <option value="color">สี</option>
            </select>

            <label for="print-type">รูปแบบการปริ้น:</label>
            <select id="print-type">
                <option value="single">หน้าเดียว</option>
                <option value="double">หน้าหลัง</option>
            </select>

            <label for="binding">รูปแบบการเข้าเล่ม:</label>
            <select id="binding">
                <option value="none">ปริ้นเฉยๆ</option>
                <option value="lac">แลคซีน</option>
                <option value="spiral">สันเกลียว</option>
                <option value="staple">เย็บมุม</option>
            </select>

            <p id="total-pages" style="margin: 10px 0; font-weight: bold;">จำนวนแผ่น: </p>
            <p id="print-price" style="margin: 10px 0; font-weight: bold;">ราคาปริ้น: </p>
            <p id="binding-price" style="margin: 10px 0; font-weight: bold;">ราคาการเข้าเล่ม: </p>
            <p id="total-price" style="margin: 10px 0; font-weight: bold;">ราคาสุทธิ: </p>

            <p class="error" id="error-message"></p>

            <button type="button" onclick="calculatePrice()">คำนวณราคา</button>
        </form>
    </div>

    <script>
        function calculatePrice() {
            const pages = parseInt(document.getElementById('pages').value);
            const paperType = document.getElementById('paper-type').value;
            const color = document.getElementById('color').value;
            const printType = document.getElementById('print-type').value;
            const binding = document.getElementById('binding').value;

            let sheets = printType === 'double' ? Math.ceil(pages / 2) : pages;
            let printPrice = 0;
            let bindingPrice = 0;
            let errorMessage = '';

            if (paperType === '70') {
                if (color === 'bw') {
                    printPrice = printType === 'double' ? sheets * 0.5 : pages * 0.5;
                } else {
                    printPrice = printType === 'double' ? sheets * 1.4 : pages * 0.75;
                }
            } else {
                if (color === 'bw') {
                    printPrice = printType === 'double' ? sheets * 1.3 : pages * 0.7;
                } else {
                    printPrice = pages;
                }
            }

            if (binding === 'lac' || binding === 'staple') {
                if (sheets > 80) {
                    errorMessage = 'จำนวนเกิน 80 แผ่นไม่สามรถเข้าเล่มแลคซีนหรือเย็บมุมได้';
                } else {
                    bindingPrice = 8;
                }
            } else if (binding === 'spiral') {
                if (sheets > 500) {
                    errorMessage = 'จำนวนเกิน 500 แผ่น ไม่สามารถเข้าเล่มได้';
                } else if (sheets <= 75) {
                    bindingPrice = 25;
                } else if (sheets <= 130) {
                    bindingPrice = 30;
                } else if (sheets <= 190) {
                    bindingPrice = 40;
                } else if (sheets <= 230) {
                    bindingPrice = 50;
                } else if (sheets <= 270) {
                    bindingPrice = 60;
                } else if (sheets <= 300) {
                    bindingPrice = 70;
                } else if (sheets <= 350) {
                    bindingPrice = 80;
                } else if (sheets <= 400) {
                    bindingPrice = 90;
                } else {
                    bindingPrice = 115;
                }
            }

            const totalPrice = printPrice + bindingPrice;

            document.getElementById('total-pages').textContent = `จำนวนแผ่น: ${sheets}`;
            document.getElementById('print-price').textContent = `ราคาปริ้น: ${printPrice.toFixed(2)} บาท`;
            document.getElementById('binding-price').textContent = `ราคาการเข้าเล่ม: ${bindingPrice} บาท`;
            document.getElementById('total-price').textContent = `ราคาสุทธิ: ${totalPrice.toFixed(2)} บาท`;

            document.getElementById('error-message').textContent = errorMessage;
        }
    </script>
</body>
</html>
