# Customers

### Customers
Replace Customers table -> /Users/patrickwaweru/development/mo/airtable-migration/Airtable/Customers Table.csv
```erDiagram
CUSTOMERS {
    uuid customer_id PK "NOT NULL"
    varchar255 application_id "nullable"
    varchar255 customer_first_name "NOT NULL"
    varchar255 customer_middle_name "nullable"
    varchar255 customer_surname_name "nullable"
    varchar20 customer_phone_number UK "NOT NULL"
    varchar255 customer_national_id_number UK "National ID - nullable"
    enum customer_gender "male, female, other - nullable"
    varchar255 customer_email_address "nullable"
    varchar20 customer_secondary_phone "nullable"
    varchar50 customer_source "airtable, other platform"
    varchar50 customer_source_id "recId"
    enum customer_status "active, suspended, closed"
    varchar500 selfie_url "nullable"
    varchar500 id_front_url "nullable"
    varchar500 id_back_url "nullable"
    varchar500 referral_code "nullable"
    text[] associated_phone_numbers "List of additional phone numbers"
    timestamp customer_created_at "NOT NULL, DEFAULT now()"
    timestamp customer_updated_at "NOT NULL, DEFAULT now()"
}

```

### Blacklist
replace airtable primary customer table -> /Users/patrickwaweru/development/mo/airtable-migration/Airtable/Primary Customer Data.csv

```
erDiagram
BLACKLIST {
    uuid id PK
    enum block_type "id_number, phone_number, imei, customer_id"
    string blocked_value "The actual ID/phone/IMEI"
    varchar50 customer_id FK "nullable - links to existing customer"
    text[] phone_numbers "Array of phone numbers associated with the block"
    jsonb associated_phone_numbers "Array of all phone numbers linked to customer"
    enum reason "fraud, default, duplicate, stolen_device, court_order, multiple_defaults, other"
    string description
    enum severity "temporary, permanent"
    boolean block_new_customers "Prevent new registrations - DEFAULT true"
    boolean block_new_applications "Prevent new sales applications - DEFAULT true"
    boolean block_existing_sales "Block payments on existing sales - DEFAULT false"
    boolean is_active "Active block - DEFAULT true"
    date expires_at "For temporary blocks - nullable"
    uuid created_by "Admin user who created block"
    uuid removed_by "Admin user who removed block - nullable"
    timestamp removed_at "When block was lifted - nullable"
    string removal_reason "Why block was removed - nullable"
    timestamp created_at "NOT NULL, DEFAULT now()"
    timestamp updated_at "NOT NULL, DEFAULT now()"
}
```

### Wallet
    
New feature, customer has total paid, for us to have new exciting features, we need to seperate them, introducing 
wallet and wallet transactions table 

    ```mermaid
     erDiagram
     CUSTOMER_WALLETS {
            uuid customer_wallet_id PK
            uuid customer_id FK
            decimal customer_wallet_available_balance "Unallocated funds"
            decimal customer_wallet_total_payments "All payments received"
            decimal customer_wallet_total_adjustments "Refunds, bonuses, penalties"
            int customer_wallet_total_days_given "Grace days granted"
            decimal customer_wallet_total_points "Loyalty/reward points"
            decimal customer_wallet_total_allocated "Money allocated to sales"
            decimal customer_wallet_lifetime_credits "All money in"
            decimal customer_wallet_lifetime_debits "All money out"
            decimal referral_code_usage_count "All money out"
            enum customer_wallet_currency "KES"
            timestamp customer_wallet_created_at
            timestamp customer_wallet_updated_at
        }
    ```
    
### Wallet Transactions
    
    ```mermaid
    erDiagram
    WALLET_TRANSACTIONS {
            uuid wallet_transaction_id PK
            uuid wallet_id FK
            uuid customer_id FK
            uuid sale_id FK "nullable - NULL for unallocated, set for sale-specific"
            uuid wallet_transaction_ref_id FK "nullable - for reversals/transfers"
            enum wallet_transaction_type "payment, allocation, adjustment, debit, transfer"
            enum wallet_transaction_sub_type "mpesa, bank, cash, refund, reversal, penalty, bonus, days_given, points, referral code usage"
            decimal wallet_transaction_amount
            decimal wallet_transaction_balance_before
            balance_after wallet_transaction_decimal
            int wallet_transaction_days_value "For days_given transactions"
            decimal wallet_transaction_points_value "For points transactions"
            enum wallet_transaction_status "pending, completed, failed, reversed"
            string reference "M-Pesa code, bank ref, etc"
            string wallet_transaction_description
            jsonb wallet_transaction_metadata "Additional flexible data"
            timestamp wallet_transaction_created_at
            timestamp wallet_transaction_updated_at
            timestamp wallet_transaction_completed_at
        }
    ```