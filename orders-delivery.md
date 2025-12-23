# Order and Delivery

### Complete table
Replace two tables in airtable

Deliveries -> /Users/patrickwaweru/development/mo/airtable-migration/Airtable/Rider App Logs from MoReach.csv
Orders -> /Users/patrickwaweru/development/mo/airtable-migration/Airtable/Deposit Deliveries Out for Delivery.csv

or interchanged I'm not sure

Notes on delivery consent otp:
```ts
@Column('jsonb', { nullable: true })
delivery_consent?: {
  otp_code?: string;
  otp_sent_at?: Date;
  otp_verified_at?: Date;
  phone_number?: string;
  consent_type?: 'otp' | 'signature' | 'photo';
  attempts?: number;
  max_attempts?: number;
};
```
```
erDiagram
    orders {
        uuid id PK
        uuid tenant_id
        uuid loan_application_id FK
        enum status
        enum payment_status
        jsonb items
        decimal total_amount
        decimal discount_amount
        varchar currency
        jsonb shipping_address
        jsonb metadata
        boolean manual_override
        timestamp created_at
        timestamp updated_at
    }

    deliveries {
        uuid id PK
        uuid order_id FK
        uuid rider_id
        jsonb provider_info
        enum status
        timestamp picked_up_at
        timestamp delivered_at
        timestamp estimated_delivery_at
        jsonb current_location
        jsonb delivery_notes
        jsonb adjustments
        decimal delivery_fee
        varchar tracking_number
        jsonb delivery_consent
        jsonb metadata
        boolean manual_override
        timestamp created_at
        timestamp updated_at
    }

    applications {
        uuid id PK
    }

    orders ||--o{ deliveries : "has"
    applications ||--o{ orders : "finances"
``