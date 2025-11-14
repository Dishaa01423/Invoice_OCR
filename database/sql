DROP TABLE IF EXISTS line_items;
DROP TABLE IF EXISTS invoices;

CREATE TABLE invoices (
    id INT AUTO_INCREMENT PRIMARY KEY,
    invoice_id VARCHAR(50) UNIQUE NOT NULL,
    invoice_number VARCHAR(50),
    invoice_date DATE,
    due_date DATE,
    vendor_name VARCHAR(255),
    vendor_address TEXT,
    buyer_name VARCHAR(255),
    buyer_address TEXT,
    gstin VARCHAR(15),
    cin VARCHAR(21),
    pan_no VARCHAR(10),
    state_code VARCHAR(50),
    place_of_supply VARCHAR(100),
    po_no VARCHAR(50),
    currency_code VARCHAR(3) DEFAULT 'INR',
    total_amount DECIMAL(12, 2),
    cgst DECIMAL(12, 2),
    sgst DECIMAL(12, 2),
    igst DECIMAL(12, 2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_invoice_number (invoice_number),
    INDEX idx_vendor_name (vendor_name),
    INDEX idx_invoice_date (invoice_date)
);

CREATE TABLE line_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    invoice_id VARCHAR(50) NOT NULL,
    description TEXT,
    quantity DECIMAL(10, 2),
    unit_price DECIMAL(12, 2),
    amount DECIMAL(12, 2),
    hsn_sac VARCHAR(20),
    unit_of_measurement VARCHAR(20),
    taxable_amount DECIMAL(12, 2),
    cgst DECIMAL(5, 2),
    cgst_amount DECIMAL(12, 2),
    sgst DECIMAL(5, 2),
    sgst_amount DECIMAL(12, 2),
    igst DECIMAL(5, 2),
    igst_amount DECIMAL(12, 2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (invoice_id) REFERENCES invoices(invoice_id) ON DELETE CASCADE,
    INDEX idx_invoice_id (invoice_id),
    INDEX idx_hsn_sac (hsn_sac)
);
