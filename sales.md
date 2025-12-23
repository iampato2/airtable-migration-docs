# Loan Engine

### Sales, Sales State and Sale Overrides

Seperate tables though
Replace this table in airtable -> /Users/patrickwaweru/development/mo/airtable-migration/Airtable/Loans table.csv
```
erDiagram
    SALES {
        uuid sale_id PK
        uuid customer_id
        uuid priceplan_id
        uuid inventory_id
        date sale_date
        enum sale_type               
        enum repayment_type          
        integer initial_weeks_to_pa
        numeric repay_rate          
        numeric total_paid           
        numeric total_discounts_deposit 
        numeric advance_lock  
        boolean returned           
        date fin_return_date        
        date last_payment_date  
        date grace_start_date
        date grace_end_date
        numeric grace_days   
        varchar sales_agent_dsr_id "Legacy DSR ID 900_Pato"
        varchar sales_agent_id "MongoDB ID (future)"
        varchar submitted_by_agent_dsr_id "Legacy DSR ID 456_Brian"
        varchar submitted_by_agent_id "Agent who submitted the sale MongoDB ID (future)"
        timestamp created_at
        timestamp updated_at
    }

    SALES_STATE {
        uuid sale_id PK "FK to SALES"
        integer customer_age_days "Days since sale_date"
        numeric initial_pay "Deposit + first n weeks"
        numeric balance "Remaining principal"
        numeric three_months_balance "Remaining after 3-month target"
        numeric six_months_balance "Remaining after 6-month target"
        numeric total_due_today "Cumulative due as of today"
        numeric arrears "Due but unpaid"
        numeric advance "Overpaid"
        date next_invoice_date "Next invoice schedule"
        date credit_expiry_date "When credit expires based on pay progress"
        integer days_past_due "DPD = current_date - credit_expiry_date"
        numeric next_due_amount "Next expected payment"
        numeric closing_balance "Balance after payoff or write-off"
        text pay_off_3_6_months "PAIDOFF_3MTH, PAIDOFF_6MTH, PAIDOFF_CASH, etc."
        text account_status "Derived from multi-condition logic"
        boolean is_in_grace_period "To flag accounts in grace window."
        timestamp updated_at "DEFAULT now()"
    }

    SALES_OVERRIDES {
        uuid override_id PK
        uuid sale_id FK
        varchar field_name
        text old_value
        text new_value
        varchar reason
        varchar approved_by
        timestamp created_at
    }
    
    SALES ||--|| SALES_STATE : maintains
    SALES ||--o{ SALES_OVERRIDES : can_be_overridden_by
```