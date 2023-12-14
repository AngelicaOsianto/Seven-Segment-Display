# Seven-Segmen-Display
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Seven Segment Display</title>
    <style>
    body 
    {
        font-family: Tahoma, sans-serif;
        background-color: rgb(131, 74, 180);
    }
        /* Gaya CSS untuk membuat tata letak halaman */
        section {
            width: 50%;
            float: left;
        }

        /* Gaya CSS untuk tujuh segmen */
        .seven-segment {
            background-color: #220e94;
            width: 120px;
            height: 180px;
            position: relative;
        }

        /* Gaya CSS untuk setiap segmen */
        .segment {
            border: 10px solid #afabab;
            width: 0;
            height: 46px;
            position: absolute;
            box-sizing: border-box;
        }

        /* Gaya CSS untuk segmen yang dinyalakan */
        .segLit {
            border-color: rgb(219, 0, 0);
        }

        /* Gaya CSS untuk elemen sebelum dan sesudah segmen */
        .segment::before,
        .segment::after {
            width: 0;
            height: 0;
            border-style: solid;
            border-width: 10px;
            border-color: transparent;
            position: absolute;
            content: '';
        }

        /* Gaya CSS untuk segmen a, g, dan d */
        #seg_g::before, #seg_a::before, #seg_d::before {
            border-left-width: 0;
            border-right-color: inherit;
            top: -10px;
            left: -20px;
        }

        #seg_g::after, #seg_a::after, #seg_d::after {
            border-right-width: 0;
            border-left-color: inherit;
            top: -10px;
            right: -20px;
        }

        /* Gaya CSS untuk segmen c, f, e, dan b */
        #seg_c::before, #seg_f::before, #seg_e::before, #seg_b::before {
            border-top-width: 0;
            border-bottom-color: inherit;
            top: -20px;
            left: -10px;
        }

        #seg_c::after, #seg_f::after, #seg_e::after, #seg_b::after {
            border-bottom-width: 0;
            border-top-color: inherit;
            bottom: -20px;
            left: -10px;
        }

        /* Gaya CSS untuk pengaturan dimensi segmen tertentu */
        #seg_g, #seg_a, #seg_d {
            width: 56px;
            height: 0;
        }

        #seg_c, #seg_f {
            right: 10px;
        }

        #seg_g, #seg_a, #seg_d {
            left: 32px;
        }

        #seg_e, #seg_b {
            left: 10px;
        }

        #seg_d {
            top: 80px;
        }

        #seg_g {
            bottom: 10px;
        }

        #seg_a {
            top: 10px;
        }

        #seg_c, #seg_b {
            top: 32px;
        }

        #seg_f, #seg_e {
            bottom: 32px;
        }

        /* Gaya CSS untuk tabel kebenaran */
        table {
            margin-top: 20px;
            border-collapse: collapse;
            width: 100%;
        }

        th, td {
            border: 1px solid #3a1492;
            padding: 8px;
            text-align: center;
        }
        th {
            background-color: #7298df;
        }
    </style>
</head>
<body>
    <!-- Judul halaman -->
    <h1>Seven Segment Display</h1>

    <!-- Konten utama halaman -->
    <main>
        <!-- Bagian input untuk memilih angka desimal -->
        <section>
            <!-- Label untuk input -->
            <label for="decimal">Decimal</label>
            <!-- Dropdown untuk memilih angka desimal -->
            <select id="decimal">
                <!-- Opsi angka dari 0 hingga 9 -->
                <option value="0">0</option>
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
                <option value="6">6</option>
                <option value="7">7</option>
                <option value="8">8</option>
                <option value="9">9</option>
            </select>
        </section>

        <!-- Bagian tujuh segmen untuk menampilkan angka -->
        <section class="seven-segment">
            <!-- Tujuh segmen (a-g) sebagai bagian dari tampilan tujuh segmen -->
            <div class="segment" id="seg_a"></div>
            <div class="segment" id="seg_b"></div>
            <div class="segment" id="seg_c"></div>
            <div class="segment" id="seg_d"></div>
            <div class="segment" id="seg_e"></div>
            <div class="segment" id="seg_f"></div>
            <div class="segment" id="seg_g"></div>
        </section>
    </main>

    <!-- Tabel kebenaran untuk menunjukkan output dari setiap segmen sesuai input -->
    <table id="truth-table">
        <!-- Script JavaScript untuk menginisialisasi dan memanipulasi tampilan -->
        <script>
            document.addEventListener('DOMContentLoaded', function () {
                // Array input dan output yang sesuai
                const inputLines = ['0000', '0001', '0010', '0011', '0100', '0101', '0110', '0111', '1000', '1001'];
                const outputLines = ['1110111', '0010010', '1011101', '1011011', '0111010', '1101011', '1101111', '1010010', '1111111', '1111011'];
        
                // Mendapatkan elemen tujuh segmen
                const segmentDisplay = getSegmentDisplay();
                // Mendapatkan elemen tabel kebenaran
                const truthTable = document.getElementById('truth-table');
        
                // Menambahkan event listener untuk perubahan pada dropdown desimal
                document.getElementById('decimal').addEventListener('change', (evt) => {
                    // Mendapatkan input yang dipilih
                    const selectedInput = inputLines[+evt.target.value];
                    // Mendapatkan output yang sesuai dengan input
                    const mappedOutput = outputLines[inputLines.indexOf(selectedInput)];
                    // Memperbarui tampilan tujuh segmen
                    updateSegmentDisplay(mappedOutput);
                    // Menampilkan baris baru pada tabel kebenaran
                    displayTruthTable(selectedInput, mappedOutput);
                });
        
                // Memperbarui tampilan tujuh segmen berdasarkan output
                function updateSegmentDisplay(outputWires) {
                    [...segmentDisplay].forEach((seg, index) => {
                        seg.classList[outputWires[index] === '1' ? 'add' : 'remove']('segLit');
                    });
                }
        
                // Mendapatkan elemen tujuh segmen
                function getSegmentDisplay() {
                    return document.querySelectorAll('.seven-segment .segment');
                }
        
                // Menampilkan baris pada tabel kebenaran
                function displayTruthTable(input, output) {
                    // Hapus baris pertama jika sudah ada
                    if (truthTable.rows.length > 1) {
                        truthTable.deleteRow(1);
                    }
        
                    // Insert baris baru
                    const row = truthTable.insertRow(-1);
                    const inputCell = row.insertCell(0);
                    inputCell.textContent = input;
        
                    // Menampilkan output pada sel-sel tabel
                    for (let i = 0; i < output.length; i++) {
                        const outputCell = row.insertCell(i + 1);
                        outputCell.textContent = output[i];
                    }
                }
            });
        </script>         
        <!-- Baris header tabel kebenaran -->
        <tr>
            <th>Input</th>
            <th>a</th>
            <th>b</th>
            <th>c</th>
            <th>d</th>
            <th>e</th>
            <th>f</th>
            <th>g</th>
        </tr>
    </table>
</body>
</html>
