### ERD
<details>
<summary>Click</summary>

</details>


---

```sql
-- DDL Schema Database Modul Keuangan & Akuntansi (Laravel/Node/Python Seeding Friendly)

-- Database Dialect: PostgreSQL / MySQL Compatible (Standard SQL DDL)

-- =========================================================================

-- 1. MASTER DATA TABLES

-- =========================================================================

CREATE TABLE account_types (

id SERIAL PRIMARY KEY,

name VARCHAR(50) NOT NULL UNIQUE,

category VARCHAR(20) NOT NULL CHECK (category IN ('asset', 'liability', 'equity', 'revenue', 'expense')),

normal_balance VARCHAR(10) NOT NULL CHECK (normal_balance IN ('debit', 'credit')),

report_type VARCHAR(20) NOT NULL CHECK (report_type IN ('balance_sheet', 'profit_loss'))

);

CREATE TABLE chart_of_accounts (

id SERIAL PRIMARY KEY,

account_code VARCHAR(20) NOT NULL UNIQUE,

account_name VARCHAR(100) NOT NULL,

account_type_id INT REFERENCES account_types(id) ON DELETE RESTRICT,

parent_id INT REFERENCES chart_of_accounts(id) ON DELETE SET NULL,

is_header BOOLEAN DEFAULT FALSE,

is_active BOOLEAN DEFAULT TRUE,

is_contra BOOLEAN DEFAULT FALSE,

current_balance DECIMAL(15, 2) DEFAULT 0.00,

sort_order INT DEFAULT 0

);

CREATE TABLE periods (

id SERIAL PRIMARY KEY,

period_name VARCHAR(50) NOT NULL,

month VARCHAR(2) NOT NULL,

year VARCHAR(4) NOT NULL,

start_date DATE NOT NULL,

end_date DATE NOT NULL,

is_closed BOOLEAN DEFAULT FALSE

);

CREATE TABLE contacts (

id SERIAL PRIMARY KEY,

name VARCHAR(100) NOT NULL,

type VARCHAR(20) NOT NULL CHECK (type IN ('customer', 'vendor', 'employee', 'other')),

phone VARCHAR(20),

email VARCHAR(100),

is_active BOOLEAN DEFAULT TRUE

);

-- =========================================================================

-- 2. DRAFT & POSTING LEDGER TABLES (General Ledger)

-- =========================================================================

CREATE TABLE journal_entries (

id SERIAL PRIMARY KEY,

ref_number VARCHAR(50) NOT NULL UNIQUE,

transaction_date DATE NOT NULL,

description TEXT,

period_id INT NOT NULL REFERENCES periods(id) ON DELETE RESTRICT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled', 'void')),

-- Polymorphic Reference to Subledger Documents

source_type VARCHAR(50), -- 'cash_transaction', 'invoice', 'bill'

source_id INT,

-- Audit Trails (FK to HRIS/RBAC User IDs in v1.0.0)

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

posted_by VARCHAR(100),

posted_at TIMESTAMP,

voided_by VARCHAR(100),

voided_at TIMESTAMP

);

CREATE TABLE journal_items (

id SERIAL PRIMARY KEY,

journal_id INT NOT NULL REFERENCES journal_entries(id) ON DELETE CASCADE,

account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

debit DECIMAL(15, 2) NOT NULL DEFAULT 0.00,

credit DECIMAL(15, 2) NOT NULL DEFAULT 0.00,

description TEXT

);

-- =========================================================================

-- 3. SUBLEDGER / SPECIAL TRANSACTION TABLES (Dokumen Sumber)

-- =========================================================================

-- Penerimaan, Pengeluaran, Modal, Prive

CREATE TABLE cash_transactions (

id SERIAL PRIMARY KEY,

type VARCHAR(20) NOT NULL CHECK (type IN ('receipt', 'payment', 'capital_in', 'prive_out')),

transaction_date DATE NOT NULL,

ref_number VARCHAR(50),

cash_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

offset_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

amount DECIMAL(15, 2) NOT NULL,

description TEXT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled')),

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

posted_by VARCHAR(100),

posted_at TIMESTAMP

);

-- Piutang

CREATE TABLE invoices (

id SERIAL PRIMARY KEY,

transaction_date DATE NOT NULL,

due_date DATE NOT NULL,

ref_number VARCHAR(50),

contact_id INT NOT NULL REFERENCES contacts(id) ON DELETE RESTRICT,

receivable_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

offset_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

amount DECIMAL(15, 2) NOT NULL,

description TEXT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled', 'paid')),

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

);

-- Hutang

CREATE TABLE bills (

id SERIAL PRIMARY KEY,

transaction_date DATE NOT NULL,

due_date DATE NOT NULL,

ref_number VARCHAR(50) NOT NULL,

contact_id INT NOT NULL REFERENCES contacts(id) ON DELETE RESTRICT,

payable_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

offset_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

amount DECIMAL(15, 2) NOT NULL,

description TEXT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled', 'paid')),

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

);

-- =========================================================================

-- 4. PERFORMANCE INDEXES

-- =========================================================================

CREATE INDEX idx_journal_items_account ON journal_items(account_id);

CREATE INDEX idx_journal_items_journal ON journal_items(journal_id);

CREATE INDEX idx_journal_entries_date ON journal_entries(transaction_date);

CREATE INDEX idx_journal_entries_source ON journal_entries(source_type, source_id);

CREATE INDEX idx_coa_code ON chart_of_accounts(account_code);

CREATE INDEX idx_invoices_due ON invoices(due_date);

CREATE INDEX idx_bills_due ON bills(due_date);
```

```sql
-- DDL Schema Database Modul Keuangan & Akuntansi (Laravel/Node/Python Seeding Friendly)

-- Database Dialect: PostgreSQL / MySQL Compatible (Standard SQL DDL)

-- =========================================================================

-- 1. MASTER DATA TABLES

-- =========================================================================

CREATE TABLE account_types (

id SERIAL PRIMARY KEY,

name VARCHAR(50) NOT NULL UNIQUE,

category VARCHAR(20) NOT NULL CHECK (category IN ('asset', 'liability', 'equity', 'revenue', 'expense')),

normal_balance VARCHAR(10) NOT NULL CHECK (normal_balance IN ('debit', 'credit')),

report_type VARCHAR(20) NOT NULL CHECK (report_type IN ('balance_sheet', 'profit_loss'))

);

CREATE TABLE chart_of_accounts (

id SERIAL PRIMARY KEY,

account_code VARCHAR(20) NOT NULL UNIQUE,

account_name VARCHAR(100) NOT NULL,

account_type_id INT REFERENCES account_types(id) ON DELETE RESTRICT,

parent_id INT REFERENCES chart_of_accounts(id) ON DELETE SET NULL,

is_header BOOLEAN DEFAULT FALSE,

is_active BOOLEAN DEFAULT TRUE,

is_contra BOOLEAN DEFAULT FALSE,

current_balance DECIMAL(15, 2) DEFAULT 0.00,

sort_order INT DEFAULT 0

);

CREATE TABLE periods (

id SERIAL PRIMARY KEY,

period_name VARCHAR(50) NOT NULL,

month VARCHAR(2) NOT NULL,

year VARCHAR(4) NOT NULL,

start_date DATE NOT NULL,

end_date DATE NOT NULL,

is_closed BOOLEAN DEFAULT FALSE

);

CREATE TABLE contacts (

id SERIAL PRIMARY KEY,

name VARCHAR(100) NOT NULL,

type VARCHAR(20) NOT NULL CHECK (type IN ('customer', 'vendor', 'employee', 'other')),

phone VARCHAR(20),

email VARCHAR(100),

is_active BOOLEAN DEFAULT TRUE

);

-- =========================================================================

-- 2. DRAFT & POSTING LEDGER TABLES (General Ledger)

-- =========================================================================

CREATE TABLE journal_entries (

id SERIAL PRIMARY KEY,

ref_number VARCHAR(50) NOT NULL UNIQUE,

transaction_date DATE NOT NULL,

description TEXT,

period_id INT NOT NULL REFERENCES periods(id) ON DELETE RESTRICT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled', 'void')),

-- Polymorphic Reference to Subledger Documents

source_type VARCHAR(50), -- 'cash_transaction', 'invoice', 'bill'

source_id INT,

-- Audit Trails (FK to HRIS/RBAC User IDs in v1.0.0)

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

posted_by VARCHAR(100),

posted_at TIMESTAMP,

voided_by VARCHAR(100),

voided_at TIMESTAMP

);

CREATE TABLE journal_items (

id SERIAL PRIMARY KEY,

journal_id INT NOT NULL REFERENCES journal_entries(id) ON DELETE CASCADE,

account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

debit DECIMAL(15, 2) NOT NULL DEFAULT 0.00,

credit DECIMAL(15, 2) NOT NULL DEFAULT 0.00,

description TEXT

);

-- =========================================================================

-- 3. SUBLEDGER / SPECIAL TRANSACTION TABLES (Dokumen Sumber)

-- =========================================================================

-- Penerimaan, Pengeluaran, Modal, Prive

CREATE TABLE cash_transactions (

id SERIAL PRIMARY KEY,

type VARCHAR(20) NOT NULL CHECK (type IN ('receipt', 'payment', 'capital_in', 'prive_out')),

transaction_date DATE NOT NULL,

ref_number VARCHAR(50),

cash_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

offset_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

amount DECIMAL(15, 2) NOT NULL,

description TEXT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled')),

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

posted_by VARCHAR(100),

posted_at TIMESTAMP

);

-- Piutang

CREATE TABLE invoices (

id SERIAL PRIMARY KEY,

transaction_date DATE NOT NULL,

due_date DATE NOT NULL,

ref_number VARCHAR(50),

contact_id INT NOT NULL REFERENCES contacts(id) ON DELETE RESTRICT,

receivable_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

offset_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

amount DECIMAL(15, 2) NOT NULL,

description TEXT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled', 'paid')),

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

);

-- Hutang

CREATE TABLE bills (

id SERIAL PRIMARY KEY,

transaction_date DATE NOT NULL,

due_date DATE NOT NULL,

ref_number VARCHAR(50) NOT NULL,

contact_id INT NOT NULL REFERENCES contacts(id) ON DELETE RESTRICT,

payable_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

offset_account_id INT NOT NULL REFERENCES chart_of_accounts(id) ON DELETE RESTRICT,

amount DECIMAL(15, 2) NOT NULL,

description TEXT,

status VARCHAR(20) NOT NULL DEFAULT 'draft' CHECK (status IN ('draft', 'posted', 'cancelled', 'paid')),

created_by VARCHAR(100) NOT NULL,

created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

);

-- =========================================================================

-- 4. PERFORMANCE INDEXES

-- =========================================================================

CREATE INDEX idx_journal_items_account ON journal_items(account_id);

CREATE INDEX idx_journal_items_journal ON journal_items(journal_id);

CREATE INDEX idx_journal_entries_date ON journal_entries(transaction_date);

CREATE INDEX idx_journal_entries_source ON journal_entries(source_type, source_id);

CREATE INDEX idx_coa_code ON chart_of_accounts(account_code);

CREATE INDEX idx_invoices_due ON invoices(due_date);

CREATE INDEX idx_bills_due ON bills(due_date);
```