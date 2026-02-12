# e-Mandi - Requirements Specification

## Project Overview

e-Mandi is an all-in-one digital marketplace that connects farmers with buyers for the sale and purchase of agricultural produce. The platform features real-time listings, order tracking, and category-based crop discovery, enabling direct farmer-to-buyer transactions.

## 1. Functional Requirements

### 1.1 User Management

#### 1.1.1 Authentication & Authorization
- Users must be able to register with email and password
- Users must be able to log in securely using JWT-based authentication
- Users must be able to log out from the system
- System must support three user roles: Farmer, Buyer, and Admin
- Users must be able to switch between Farmer and Buyer roles dynamically

#### 1.1.2 User Profile Management
- Users must be able to create and update their profile information including:
  - Full name
  - Email address
  - Phone number
  - Address (street, city, state, pincode)
- Profile information must be persisted and retrievable across sessions

### 1.2 Farmer Features

#### 1.2.1 Crop Listing Management
- Farmers must be able to add new crop listings with the following details:
  - Crop name
  - Category (vegetables, fruits, grains, pulses, spices, dairy, other)
  - Description
  - Quantity available (in kg or specified unit)
  - Price per unit
  - Harvest date
  - Expiry date
  - Location
  - Image URL
  - Organic certification status
- Farmers must be able to edit their existing crop listings
- Farmers must be able to delete their crop listings
- Farmers must be able to mark crops as available or unavailable
- System must automatically hide crops with zero quantity

#### 1.2.2 Order Management (Farmer Side)
- Farmers must be able to view all orders placed for their crops
- Farmers must be able to see order details including:
  - Buyer information (name, contact details)
  - Crop name and quantity ordered
  - Total price
  - Delivery address
  - Order status
  - Order date
- Farmers must be able to update order status through the following workflow:
  - Pending → Confirmed → Shipped → Delivered
  - Ability to cancel orders
- Farmers must receive real-time notifications for new orders

#### 1.2.3 Farmer Dashboard
- Dashboard must display key metrics:
  - Total crops listed
  - Total revenue generated
  - Number of pending orders
- Dashboard must show a list of all active crop listings
- Dashboard must display recent orders with status tracking

### 1.3 Buyer Features

#### 1.3.1 Crop Discovery & Search
- Buyers must be able to browse all available crops
- Buyers must be able to search crops by:
  - Crop name
  - Description
  - Location
- Buyers must be able to filter crops by category:
  - Vegetables
  - Fruits
  - Grains
  - Pulses
  - Spices
  - Dairy
  - Other
- System must display only available crops with quantity > 0

#### 1.3.2 Crop Details & Farmer Information
- Buyers must be able to view detailed crop information including:
  - Crop name, category, and description
  - Available quantity and unit
  - Price per unit
  - Harvest and expiry dates
  - Organic certification status
  - Crop image
- Buyers must be able to view farmer information:
  - Farmer name
  - Location (city, state)
  - Contact phone number

#### 1.3.3 Order Placement
- Buyers must be able to place orders by specifying:
  - Quantity to purchase
  - Delivery address
  - Optional notes
- System must automatically calculate total price based on quantity and unit price
- System must validate that ordered quantity does not exceed available quantity
- System must update crop quantity after successful order placement

#### 1.3.4 Order Tracking (Buyer Side)
- Buyers must be able to view all their placed orders
- Buyers must be able to track order status in real-time:
  - Pending
  - Confirmed
  - Shipped
  - Delivered
  - Cancelled
- Buyers must be able to view order history with complete details

#### 1.3.5 Buyer Dashboard
- Dashboard must display key metrics:
  - Total orders placed
  - Total amount spent
  - Number of pending orders
- Dashboard must show available crops with search and filter capabilities
- Dashboard must display order history with status tracking

### 1.4 Admin Features

#### 1.4.1 Platform Analytics
- Admin must be able to view comprehensive platform statistics:
  - Total registered users
  - Number of farmers
  - Number of buyers
  - Total crops listed
  - Total orders placed
  - Pending orders count
  - Total revenue generated (from delivered orders)

#### 1.4.2 User Management
- Admin must be able to view all registered users with details:
  - Name, email, user type
  - Location information
  - Registration date
- Admin must be able to search users by name or email
- Admin must be able to view individual user profiles
- Admin must be able to suspend or ban users (future enhancement)

#### 1.4.3 Crop Management
- Admin must be able to view all crop listings across the platform
- Admin must be able to search crops by name or category
- Admin must be able to view crop details including farmer information
- Admin must be able to delete inappropriate or fraudulent crop listings
- Admin must be able to view crop availability status

#### 1.4.4 Order Management
- Admin must be able to view all orders across the platform
- Admin must be able to search orders by crop name, buyer, or farmer
- Admin must be able to view complete order details
- Admin must be able to update order status for dispute resolution
- Admin must be able to monitor pending orders requiring attention

#### 1.4.5 Access Control
- Admin dashboard must be accessible only to users with admin privileges
- System must verify admin status before granting access
- Non-admin users must be denied access with appropriate error message

### 1.5 Notification System
- Users must receive toast notifications for:
  - Successful operations (order placed, crop added, status updated)
  - Error conditions (failed operations, validation errors)
  - Important updates (order status changes)

## 2. Non-Functional Requirements

### 2.1 Performance
- Page load time must not exceed 3 seconds under normal network conditions
- Search and filter operations must return results within 1 second
- Database queries must be optimized with appropriate indexes
- System must handle concurrent users without performance degradation
- Images must be optimized for web delivery

### 2.2 Security

#### 2.2.1 Authentication & Authorization
- All passwords must be hashed using bcrypt before storage
- JWT tokens must be used for session management
- Tokens must expire after a reasonable time period
- All API endpoints must require authentication
- Role-based access control (RBAC) must be enforced at database level

#### 2.2.2 Data Protection
- Row Level Security (RLS) must be enabled on all database tables
- Users must only access their own data unless explicitly authorized
- Farmers can only modify their own crop listings
- Buyers can only view their own orders
- Admin access must be strictly controlled and audited

#### 2.2.3 Input Validation
- All user inputs must be validated on both client and server side
- SQL injection prevention through parameterized queries
- XSS prevention through input sanitization
- CSRF protection must be implemented

### 2.3 Scalability
- Database schema must support horizontal scaling
- Application must be stateless to enable load balancing
- Caching strategies must be implemented for frequently accessed data
- System must handle increasing number of users and transactions

### 2.4 Reliability & Availability
- System uptime must be at least 99.5%
- Database backups must be performed daily
- System must gracefully handle errors without crashing
- Failed transactions must be rolled back to maintain data consistency

### 2.5 Usability
- User interface must be intuitive and easy to navigate
- System must be responsive and work on desktop, tablet, and mobile devices
- Forms must provide clear validation messages
- Loading states must be displayed during asynchronous operations
- Error messages must be user-friendly and actionable

### 2.6 Maintainability
- Code must follow consistent coding standards and conventions
- Components must be modular and reusable
- Database schema must be version-controlled through migrations
- System must have comprehensive error logging
- Code must be well-documented with comments where necessary

### 2.7 Compatibility
- Application must work on modern browsers:
  - Chrome (latest 2 versions)
  - Firefox (latest 2 versions)
  - Safari (latest 2 versions)
  - Edge (latest 2 versions)
- Application must be mobile-responsive
- System must support different screen sizes and resolutions

### 2.8 Data Integrity
- All database operations must maintain ACID properties
- Foreign key constraints must be enforced
- Cascade deletes must be properly configured
- Timestamps (created_at, updated_at) must be automatically managed
- Quantity validation must prevent negative values or overselling

### 2.9 Accessibility
- Application should follow WCAG 2.1 guidelines where possible
- Proper semantic HTML must be used
- Keyboard navigation must be supported
- Color contrast ratios must meet accessibility standards
- Screen reader compatibility should be considered

### 2.10 Monitoring & Logging
- System must log all critical operations
- Error tracking must be implemented
- Performance metrics must be monitored
- User activity must be logged for audit purposes
- Database query performance must be monitored

## 3. Technical Constraints

### 3.1 Technology Stack
- Frontend: React.js with TypeScript
- UI Framework: Tailwind CSS with shadcn/ui components
- Backend: Supabase (PostgreSQL database with built-in authentication)
- State Management: React Query (@tanstack/react-query)
- Routing: React Router DOM
- Form Handling: React Hook Form with Zod validation
- Build Tool: Vite

### 3.2 Database
- PostgreSQL database through Supabase
- Row Level Security (RLS) policies must be defined
- Database migrations must be version-controlled
- Indexes must be created for frequently queried columns

### 3.3 Deployment
- Application must be deployable to modern hosting platforms
- Environment variables must be used for configuration
- Production build must be optimized and minified

## 4. Data Models

### 4.1 User Profile
- user_id (UUID, Primary Key)
- email (Text, Unique)
- full_name (Text)
- user_type (Enum: farmer, buyer, admin)
- phone (Text)
- address (Text)
- city (Text)
- state (Text)
- pincode (Text)
- created_at (Timestamp)
- updated_at (Timestamp)

### 4.2 Crop
- id (UUID, Primary Key)
- farmer_id (UUID, Foreign Key → auth.users)
- name (Text)
- category (Enum: vegetables, fruits, grains, pulses, spices, dairy, other)
- description (Text)
- quantity_available (Decimal)
- unit (Text, default: 'kg')
- price_per_unit (Decimal)
- harvest_date (Date)
- expiry_date (Date)
- location (Text)
- image_url (Text)
- is_available (Boolean)
- is_organic (Boolean)
- created_at (Timestamp)
- updated_at (Timestamp)

### 4.3 Order
- id (UUID, Primary Key)
- buyer_id (UUID, Foreign Key → auth.users)
- farmer_id (UUID, Foreign Key → auth.users)
- crop_id (UUID, Foreign Key → crops)
- quantity (Decimal)
- total_price (Decimal)
- status (Enum: pending, confirmed, shipped, delivered, cancelled)
- delivery_address (Text)
- delivery_date (Date)
- notes (Text)
- created_at (Timestamp)
- updated_at (Timestamp)

## 5. User Workflows

### 5.1 Farmer Workflow
1. Register/Login as Farmer
2. Complete profile with location and contact details
3. Add crop listings with details and pricing
4. Receive orders from buyers
5. Update order status as processing progresses
6. Track revenue and manage inventory

### 5.2 Buyer Workflow
1. Register/Login as Buyer
2. Browse available crops with search and filters
3. View crop details and farmer information
4. Place order with quantity and delivery address
5. Track order status in real-time
6. View order history and spending

### 5.3 Admin Workflow
1. Login with admin credentials
2. Monitor platform analytics and metrics
3. Review and manage user accounts
4. Moderate crop listings for quality control
5. Resolve order disputes and issues
6. Generate reports on platform activity

## 6. Future Enhancements

### 6.1 Payment Integration
- Integrate payment gateway for online transactions
- Support multiple payment methods
- Implement escrow system for secure transactions

### 6.2 Rating & Review System
- Allow buyers to rate farmers and crops
- Display ratings on crop listings
- Implement review moderation

### 6.3 Advanced Search
- Implement geolocation-based search
- Add price range filters
- Support sorting by various criteria

### 6.4 Messaging System
- Enable direct communication between farmers and buyers
- Implement in-app chat functionality
- Support order-related queries

### 6.5 Mobile Application
- Develop native mobile apps for iOS and Android
- Push notifications for order updates
- Offline capability for viewing listings

### 6.6 Analytics Dashboard
- Advanced reporting for farmers
- Sales trends and insights
- Demand forecasting

### 6.7 Multi-language Support
- Support regional languages
- Localization for different markets

## 7. Success Metrics

- Number of registered farmers and buyers
- Total crops listed on the platform
- Order completion rate
- Average order value
- User retention rate
- Platform revenue
- User satisfaction scores
- Response time for order processing

---

**Document Version:** 1.0  
**Last Updated:** February 12, 2026  
**Status:** Active Development
