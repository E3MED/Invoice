<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>E3MED Professional Invoice System</title>
  <style>
    body { 
      font-family: Calibri, Arial, sans-serif; 
      margin: 0; 
      padding: 20px; 
      background-color: #f5f5f5;
    }
    .invoice-container {
      max-width: 800px;
      margin: 0 auto;
      background: white;
      padding: 30px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .header {
      display: flex;
      justify-content: space-between;
      margin-bottom: 30px;
      border-bottom: 2px solid #2F75B5;
      padding-bottom: 20px;
    }
    .logo {
      color: #2F75B5;
      font-size: 24px;
      font-weight: bold;
      margin-bottom: 5px;
    }
    .company-info {
      font-size: 11px;
      line-height: 1.5;
    }
    .invoice-title {
      font-size: 28px;
      color: #333;
      text-align: right;
    }
    .invoice-info {
      display: flex;
      justify-content: space-between;
      margin-bottom: 30px;
    }
    .client-info, .invoice-meta {
      width: 48%;
    }
    .info-label {
      font-weight: bold;
      display: inline-block;
      width: 120px;
    }
    .items-table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 20px;
    }
    .items-table th {
      background-color: #2F75B5;
      color: white;
      padding: 10px;
      text-align: left;
    }
    .items-table td {
      padding: 10px;
      border-bottom: 1px solid #ddd;
    }
    .items-table tr:last-child td {
      border-bottom: 2px solid #2F75B5;
    }
    .totals {
      float: right;
      width: 300px;
      margin-top: 20px;
    }
    .total-row {
      display: flex;
      justify-content: space-between;
      margin-bottom: 10px;
    }
    .footer {
      margin-top: 50px;
      padding-top: 20px;
      border-top: 1px solid #ddd;
      font-size: 11px;
      text-align: center;
    }
    input, select {
      padding: 8px;
      margin: 5px 0;
      border: 1px solid #ddd;
      width: 100%;
      box-sizing: border-box;
    }
    button {
      padding: 10px 15px;
      background-color: #2F75B5;
      color: white;
      border: none;
      cursor: pointer;
      margin-right: 10px;
      margin-top: 20px;
    }
    button:hover {
      background-color: #1a5a8f;
    }
    #loginSection {
      max-width: 300px;
      margin: 50px auto;
      padding: 20px;
      background: white;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    #savedInvoices {
      margin-top: 40px;
    }
    .saved-invoice {
      background: #f9f9f9;
      padding: 15px;
      margin-bottom: 15px;
      border-left: 4px solid #2F75B5;
    }
  </style>
</head>
<body>

<!-- Login Section -->
<div id="loginSection">
  <h2>🔒 E3MED Invoice Login</h2>
  <input type="text" id="username" placeholder="Username" value="e3med"><br>
  <input type="password" id="password" placeholder="Password" value="e3med2025+"><br>
  <button onclick="checkLogin()">Login</button>
  <p id="loginMessage" style="color:red;"></p>
</div>

<!-- Invoice Section -->
<div id="invoiceSection" style="display:none;">
  <div class="invoice-container">
    <!-- Header -->
    <div class="header">
      <div>
        <div class="logo">E3 MED</div>
        <div class="company-info">
          Kaslik Main Street - Debs Center - 4th floor<br>
          Keserwan - Lebanon<br>
          Tel: 96181804661<br>
          Email: management@e3-med.com<br>
          FR: 3970535
        </div>
      </div>
      <div class="invoice-title">INVOICE</div>
    </div>

    <!-- Invoice Info -->
    <div class="invoice-info">
      <div class="client-info">
        <div><span class="info-label">Bill To:</span>
          <input type="text" id="customerName" placeholder="Customer Name">
        </div>
        <div><span class="info-label">Address:</span>
          <input type="text" id="customerAddress" placeholder="Customer Address">
        </div>
      </div>
      <div class="invoice-meta">
        <div><span class="info-label">Invoice #:</span>
          <input type="text" id="invoiceNumber" readonly>
        </div>
        <div><span class="info-label">Date:</span>
          <input type="date" id="invoiceDate">
        </div>
        <div><span class="info-label">Terms:</span>
          <input type="text" id="terms" placeholder="Payment terms">
        </div>
        <div><span class="info-label">Currency:</span>
          <select id="currency">
            <option value="USD">USD</option>
            <option value="EURO">EURO</option>
          </select>
        </div>
      </div>
    </div>

    <!-- Items Table -->
    <table class="items-table">
      <thead>
        <tr>
          <th style="width:10%">REF</th>
          <th style="width:40%">DESCRIPTION</th>
          <th style="width:10%">QTY</th>
          <th style="width:15%">UNIT PRICE</th>
          <th style="width:15%">AMOUNT</th>
          <th style="width:10%"></th>
        </tr>
      </thead>
      <tbody id="invoiceItems">
        <!-- Rows will be added here -->
      </tbody>
    </table>

    <button onclick="addRow()">+ Add Item</button>

    <!-- Totals -->
    <div class="totals">
      <div class="total-row">
        <div>Subtotal:</div>
        <div><span id="subTotal">0.00</span> <span id="currencyDisplay">USD</span></div>
      </div>
      <div class="total-row">
        <div>Discount:</div>
        <div><input type="number" id="discount" value="0" style="width:80px; display:inline-block;" onchange="calculateTotal()"> <span id="currencyDisplay2">USD</span></div>
      </div>
      <div class="total-row" style="font-weight:bold; font-size:1.1em;">
        <div>TOTAL:</div>
        <div><span id="totalAmount">0.00</span> <span id="currencyDisplay3">USD</span></div>
      </div>
    </div>

    <div style="clear:both;"></div>

    <!-- Footer -->
    <div class="footer">
      If you have any questions about this invoice, please contact<br>
      E3 MED - Lebanon - Keserwan- Kaslik Main Street - Debs Center - 4th Floor<br>
      Tel: 96181804661 - Email: management@e3-med.com
    </div>

    <!-- Action Buttons -->
    <button onclick="saveInvoice()">Save Invoice</button>
    <button onclick="generatePDF()">Download PDF</button>
    <button onclick="clearForm()">New Invoice</button>
  </div>

  <!-- Saved Invoices -->
  <div id="savedInvoices">
    <h3>Saved Invoices</h3>
    <div id="invoicesList"></div>
  </div>
</div>

<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-database-compat.js"></script>

<!-- html2pdf library -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

<script>
  // Your Firebase configuration
  const firebaseConfig = {
    apiKey: "AIzaSyBjZjNOWQwREWbNiPK5PTUHqDkiWuEim_g",
    authDomain: "e3med-finance.firebaseapp.com",
    databaseURL: "https://e3med-finance-default-rtdb.europe-west1.firebasedatabase.app",
    projectId: "e3med-finance",
    storageBucket: "e3med-finance.firebasestorage.app",
    messagingSenderId: "386561578213",
    appId: "1:386561578213:web:2dffa095390d44c2ef700c",
    measurementId: "G-QFHGC17P36"
  };

  // Initialize Firebase
  const app = firebase.initializeApp(firebaseConfig);
  const database = firebase.database();

  let invoices = [];
  let nextInvoiceNumber = 1;

  // Initialize the page
  function init() {
    // Set today's date as default
    document.getElementById('invoiceDate').valueAsDate = new Date();
    
    // Add initial rows
    for (let i = 0; i < 5; i++) {
      addRow();
    }
    
    // Load saved invoices
    loadInvoices();
    
    // Generate invoice number
    generateInvoiceNumber();
    
    // Currency change handler
    document.getElementById('currency').addEventListener('change', function() {
      const currency = this.value;
      document.querySelectorAll('#currencyDisplay, #currencyDisplay2, #currencyDisplay3').forEach(el => {
        el.textContent = currency;
      });
    });
    
    // Initialize invoice counter
    initInvoiceCounter();
  }

  function initInvoiceCounter() {
    database.ref('invoiceCounter').once('value').then(snapshot => {
      if (!snapshot.exists()) {
        database.ref('invoiceCounter').set(1);
      }
    });
  }

  function checkLogin() {
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;
    
    if (username === "e3med" && password === "e3med2025+") {
      document.getElementById('loginSection').style.display = 'none';
      document.getElementById('invoiceSection').style.display = 'block';
      init();
    } else {
      document.getElementById('loginMessage').textContent = "Invalid credentials!";
    }
  }

  function addRow() {
    const tbody = document.getElementById('invoiceItems');
    const rowId = Date.now();
    const row = document.createElement('tr');
    row.setAttribute('data-row-id', rowId);
    
    row.innerHTML = `
      <td><input type="text" class="row-ref" data-row="${rowId}" placeholder="REF"></td>
      <td><input type="text" class="row-desc" data-row="${rowId}" placeholder="Description"></td>
      <td><input type="number" class="row-qty" data-row="${rowId}" value="1" min="0" step="1" onchange="calculateRowAmount('${rowId}')"></td>
      <td><input type="number" class="row-price" data-row="${rowId}" value="0.00" min="0" step="0.01" onchange="calculateRowAmount('${rowId}')"></td>
      <td class="row-amount" data-row="${rowId}">0.00</td>
      <td><button onclick="removeRow('${rowId}')" style="background:red; padding:5px 8px;">×</button></td>
    `;
    
    tbody.appendChild(row);
  }

  function removeRow(rowId) {
    const row = document.querySelector(`tr[data-row-id="${rowId}"]`);
    if (row) {
      row.remove();
      calculateTotal();
    }
  }

  function calculateRowAmount(rowId) {
    const qty = parseFloat(document.querySelector(`.row-qty[data-row="${rowId}"]`).value) || 0;
    const price = parseFloat(document.querySelector(`.row-price[data-row="${rowId}"]`).value) || 0;
    const amount = qty * price;
    
    document.querySelector(`.row-amount[data-row="${rowId}"]`).textContent = amount.toFixed(2);
    calculateTotal();
  }

  function calculateTotal() {
    const amounts = Array.from(document.querySelectorAll('.row-amount')).map(el => parseFloat(el.textContent) || 0);
    const subTotal = amounts.reduce((sum, amount) => sum + amount, 0);
    const discount = parseFloat(document.getElementById('discount').value) || 0;
    const total = subTotal - discount;
    
    document.getElementById('subTotal').textContent = subTotal.toFixed(2);
    document.getElementById('totalAmount').textContent = total.toFixed(2);
  }

  function generateInvoiceNumber() {
    const now = new Date();
    const year = now.getFullYear();
    const month = String(now.getMonth() + 1).padStart(2, '0');
    const day = String(now.getDate()).padStart(2, '0');
    
    database.ref('invoiceCounter').once('value').then(snapshot => {
      let counter = snapshot.val() || 1;
      document.getElementById('invoiceNumber').value = `INV-${year}${month}${day}-${String(counter).padStart(4, '0')}`;
      nextInvoiceNumber = counter + 1;
    });
  }

  function saveInvoice() {
    const items = [];
    const rows = document.querySelectorAll('#invoiceItems tr');
    
    rows.forEach(row => {
      const rowId = row.getAttribute('data-row-id');
      items.push({
        reference: row.querySelector(`.row-ref[data-row="${rowId}"]`).value,
        description: row.querySelector(`.row-desc[data-row="${rowId}"]`).value,
        quantity: parseFloat(row.querySelector(`.row-qty[data-row="${rowId}"]`).value) || 0,
        unitPrice: parseFloat(row.querySelector(`.row-price[data-row="${rowId}"]`).value) || 0,
        amount: parseFloat(row.querySelector(`.row-amount[data-row="${rowId}"]`).textContent) || 0
      });
    });
    
    const invoice = {
      date: document.getElementById('invoiceDate').value,
      invoiceNumber: document.getElementById('invoiceNumber').value,
      customerName: document.getElementById('customerName').value,
      customerAddress: document.getElementById('customerAddress').value,
      terms: document.getElementById('terms').value,
      currency: document.getElementById('currency').value,
      items: items,
      subTotal: parseFloat(document.getElementById('subTotal').textContent) || 0,
      discount: parseFloat(document.getElementById('discount').value) || 0,
      total: parseFloat(document.getElementById('totalAmount').textContent) || 0,
      timestamp: firebase.database.ServerValue.TIMESTAMP
    };
    
    // Push to Firebase
    database.ref('invoices').push(invoice)
      .then(() => {
        // Update invoice counter
        database.ref('invoiceCounter').set(nextInvoiceNumber)
          .then(() => {
            alert('Invoice saved successfully!');
            loadInvoices();
            generateInvoiceNumber();
          });
      })
      .catch(error => {
        alert('Error saving invoice: ' + error.message);
      });
  }

  function loadInvoices() {
    document.getElementById('invoicesList').innerHTML = '<p>Loading invoices...</p>';
    
    database.ref('invoices').orderByChild('timestamp').once('value')
      .then(snapshot => {
        invoices = [];
        snapshot.forEach(childSnapshot => {
          invoices.push({
            id: childSnapshot.key,
            ...childSnapshot.val()
          });
        });
        renderInvoices();
      })
      .catch(error => {
        console.error("Error loading invoices:", error);
        document.getElementById('invoicesList').innerHTML = '<p>Error loading invoices. Please try again.</p>';
      });
  }

  function renderInvoices() {
    const container = document.getElementById('invoicesList');
    
    if (invoices.length === 0) {
      container.innerHTML = '<p>No saved invoices found.</p>';
      return;
    }
    
    // Sort invoices by date (newest first)
    invoices.sort((a, b) => b.timestamp - a.timestamp);
    
    let html = '';
    invoices.forEach(invoice => {
      html += `
        <div class="saved-invoice" data-id="${invoice.id}">
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <div>
              <strong>${invoice.invoiceNumber}</strong><br>
              <span>${invoice.date} - ${invoice.customerName}</span>
            </div>
            <div style="text-align:right;">
              <strong>${invoice.total.toFixed(2)} ${invoice.currency}</strong><br>
              <div style="margin-top:5px;">
                <button onclick="loadInvoiceToForm('${invoice.id}')" style="background:#2F75B5; padding:3px 8px; font-size:12px;">Edit</button>
                <button onclick="downloadInvoicePDF('${invoice.id}')" style="background:green; padding:3px 8px; font-size:12px;">PDF</button>
                <button onclick="deleteInvoice('${invoice.id}')" style="background:red; padding:3px 8px; font-size:12px;">Delete</button>
              </div>
            </div>
          </div>
        </div>
      `;
    });
    
    container.innerHTML = html;
  }

  function loadInvoiceToForm(invoiceId) {
    const invoice = invoices.find(i => i.id === invoiceId);
    if (!invoice) return;
    
    // Fill the form with the invoice data
    document.getElementById('invoiceDate').value = invoice.date;
    document.getElementById('invoiceNumber').value = invoice.invoiceNumber;
    document.getElementById('customerName').value = invoice.customerName;
    document.getElementById('customerAddress').value = invoice.customerAddress;
    document.getElementById('terms').value = invoice.terms;
    document.getElementById('currency').value = invoice.currency;
    document.getElementById('discount').value = invoice.discount;
    
    // Update currency displays
    document.querySelectorAll('#currencyDisplay, #currencyDisplay2, #currencyDisplay3').forEach(el => {
      el.textContent = invoice.currency;
    });
    
    // Clear existing rows
    document.getElementById('invoiceItems').innerHTML = '';
    
    // Add rows for each item
    invoice.items.forEach(item => {
      const rowId = Date.now();
      const row = document.createElement('tr');
      row.setAttribute('data-row-id', rowId);
      row.innerHTML = `
        <td><input type="text" class="row-ref" data-row="${rowId}" value="${item.reference || ''}"></td>
        <td><input type="text" class="row-desc" data-row="${rowId}" value="${item.description || ''}"></td>
        <td><input type="number" class="row-qty" data-row="${rowId}" value="${item.quantity}" min="0" step="1" onchange="calculateRowAmount('${rowId}')"></td>
        <td><input type="number" class="row-price" data-row="${rowId}" value="${item.unitPrice}" min="0" step="0.01" onchange="calculateRowAmount('${rowId}')"></td>
        <td class="row-amount" data-row="${rowId}">${item.amount.toFixed(2)}</td>
        <td><button onclick="removeRow('${rowId}')" style="background:red; padding:5px 8px;">×</button></td>
      `;
      document.getElementById('invoiceItems').appendChild(row);
    });
    
    // Update totals
    document.getElementById('subTotal').textContent = invoice.subTotal.toFixed(2);
    document.getElementById('totalAmount').textContent = invoice.total.toFixed(2);
    
    // Scroll to top
    window.scrollTo(0, 0);
  }

  function deleteInvoice(invoiceId) {
    if (confirm('Are you sure you want to delete this invoice?')) {
      database.ref('invoices/' + invoiceId).remove()
        .then(() => {
          alert('Invoice deleted successfully!');
          loadInvoices();
        })
        .catch(error => {
          alert('Error deleting invoice: ' + error.message);
        });
    }
  }

  function clearForm() {
    document.getElementById('customerName').value = '';
    document.getElementById('customerAddress').value = '';
    document.getElementById('terms').value = '';
    document.getElementById('discount').value = '0';
    document.getElementById('invoiceItems').innerHTML = '';
    
    // Add default rows
    for (let i = 0; i < 5; i++) {
      addRow();
    }
    
    // Generate new invoice number
    generateInvoiceNumber();
    
    // Reset totals
    document.getElementById('subTotal').textContent = '0.00';
    document.getElementById('totalAmount').textContent = '0.00';
  }

  function generatePDF() {
    // Clone the invoice container
    const element = document.querySelector('.invoice-container').cloneNode(true);
    
    // Remove buttons and inputs
    element.querySelectorAll('button, input, select').forEach(el => {
      if (el.id !== 'invoiceNumber' && el.id !== 'invoiceDate') {
        // Replace inputs with their values
        const span = document.createElement('span');
        span.textContent = el.value || el.textContent;
        if (el.classList.contains('row-amount')) {
          span.textContent = parseFloat(el.textContent).toFixed(2);
        }
        el.parentNode.replaceChild(span, el);
      } else {
        // Style readonly fields
        el.style.border = 'none';
        el.style.backgroundColor = 'transparent';
        el.style.padding = '0';
      }
    });
    
    // Remove the "Add Item" button
    const addButton = element.querySelector('button[onclick="addRow()"]');
    if (addButton) addButton.remove();
    
    // Remove delete buttons from items
    element.querySelectorAll('button[onclick^="removeRow"]').forEach(btn => btn.remove());
    
    // Add PDF-specific styling
    element.style.padding = '20px';
    element.style.boxShadow = 'none';
    element.style.maxWidth = '100%';
    
    // Generate PDF
    const opt = {
      margin: 10,
      filename: document.getElementById('invoiceNumber').value + '.pdf',
      image: { type: 'jpeg', quality: 0.98 },
      html2canvas: { scale: 2 },
      jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
    };
    
    html2pdf().set(opt).from(element).save();
  }

  function downloadInvoicePDF(invoiceId) {
    const invoice = invoices.find(i => i.id === invoiceId);
    if (!invoice) return;
    
    // Create a temporary element with the invoice data
    const element = document.createElement('div');
    element.style.padding = '20px';
    element.style.fontFamily = 'Calibri, Arial, sans-serif';
    
    element.innerHTML = `
      <div class="header" style="display:flex; justify-content:space-between; margin-bottom:30px; border-bottom:2px solid #2F75B5; padding-bottom:20px;">
        <div>
          <div class="logo" style="color:#2F75B5; font-size:24px; font-weight:bold; margin-bottom:5px;">E3 MED</div>
          <div class="company-info" style="font-size:11px; line-height:1.5;">
            Kaslik Main Street - Debs Center - 4th floor<br>
            Keserwan - Lebanon<br>
            Tel: 96181804661<br>
            Email: management@e3-med.com<br>
            FR: 3970535
          </div>
        </div>
        <div class="invoice-title" style="font-size:28px; color:#333; text-align:right;">INVOICE</div>
      </div>

      <div class="invoice-info" style="display:flex; justify-content:space-between; margin-bottom:30px;">
        <div class="client-info" style="width:48%;">
          <div><span style="font-weight:bold;">Bill To:</span> ${invoice.customerName}</div>
          <div><span style="font-weight:bold;">Address:</span> ${invoice.customerAddress}</div>
        </div>
        <div class="invoice-meta" style="width:48%;">
          <div><span style="font-weight:bold;">Invoice #:</span> ${invoice.invoiceNumber}</div>
          <div><span style="font-weight:bold;">Date:</span> ${invoice.date}</div>
          <div><span style="font-weight:bold;">Terms:</span> ${invoice.terms}</div>
          <div><span style="font-weight:bold;">Currency:</span> ${invoice.currency}</div>
        </div>
      </div>

      <table class="items-table" style="width:100%; border-collapse:collapse; margin-bottom:20px;">
        <thead>
          <tr>
            <th style="width:10%; background-color:#2F75B5; color:white; padding:10px; text-align:left;">REF</th>
            <th style="width:40%; background-color:#2F75B5; color:white; padding:10px; text-align:left;">DESCRIPTION</th>
            <th style="width:10%; background-color:#2F75B5; color:white; padding:10px; text-align:left;">QTY</th>
            <th style="width:15%; background-color:#2F75B5; color:white; padding:10px; text-align:left;">UNIT PRICE</th>
            <th style="width:15%; background-color:#2F75B5; color:white; padding:10px; text-align:left;">AMOUNT</th>
          </tr>
        </thead>
        <tbody>
          ${invoice.items.map(item => `
            <tr>
              <td style="padding:10px; border-bottom:1px solid #ddd;">${item.reference || ''}</td>
              <td style="padding:10px; border-bottom:1px solid #ddd;">${item.description || ''}</td>
              <td style="padding:10px; border-bottom:1px solid #ddd;">${item.quantity}</td>
              <td style="padding:10px; border-bottom:1px solid #ddd;">${item.unitPrice.toFixed(2)}</td>
              <td style="padding:10px; border-bottom:1px solid #ddd;">${item.amount.toFixed(2)}</td>
            </tr>
          `).join('')}
        </tbody>
      </table>

      <div class="totals" style="float:right; width:300px; margin-top:20px;">
        <div class="total-row" style="display:flex; justify-content:space-between; margin-bottom:10px;">
          <div>Subtotal:</div>
          <div>${invoice.subTotal.toFixed(2)} ${invoice.currency}</div>
        </div>
        <div class="total-row" style="display:flex; justify-content:space-between; margin-bottom:10px;">
          <div>Discount:</div>
          <div>${invoice.discount.toFixed(2)} ${invoice.currency}</div>
        </div>
        <div class="total-row" style="display:flex; justify-content:space-between; margin-bottom:10px; font-weight:bold; font-size:1.1em;">
          <div>TOTAL:</div>
          <div>${invoice.total.toFixed(2)} ${invoice.currency}</div>
        </div>
      </div>

      <div style="clear:both;"></div>

      <div class="footer" style="margin-top:50px; padding-top:20px; border-top:1px solid #ddd; font-size:11px; text-align:center;">
        If you have any questions about this invoice, please contact<br>
        E3 MED - Lebanon - Keserwan- Kaslik Main Street - Debs Center - 4th Floor<br>
        Tel: 96181804661 - Email: management@e3-med.com
      </div>
    `;
    
    // Generate PDF
    const opt = {
      margin: 10,
      filename: invoice.invoiceNumber + '.pdf',
      image: { type: 'jpeg', quality: 0.98 },
      html2canvas: { scale: 2 },
      jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
    };
    
    html2pdf().set(opt).from(element).save();
  }
</script>
</body>
</html>
