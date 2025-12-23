# Customers Experience

### Reviews
Replace Reviews table ->/Users/patrickwaweru/development/mo/airtable-migration/Airtable/Customer Reviews Grid View.csv
with future proofed to take in more features 

```
erDiagram
    reviews {
        uuid id PK
        varchar(50) reviewable_type "phone, onboarding, support_ticket, etc"
        varchar(255) reviewable_id "ID of entity being reviewed"
        integer rating "1-5 stars"
        varchar(255) title "nullable, predefined or custom"
        text comment
        uuid reviewer_id FK
        varchar(50) reviewer_type "customer, agent, staff"
        varchar(255) reviewer_name "denormalized"
        boolean is_verified "verified purchase/interaction"
        boolean is_featured "highlight good reviews"
        text response_text
        uuid response_by FK
        timestamptz response_at
        integer helpful_count
        integer not_helpful_count
        varchar(50) status "pending, approved, rejected"
        text moderation_notes
        jsonb metadata "flexible context data"
        timestamptz created_at
        timestamptz updated_at
    }
```
