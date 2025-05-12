# AirBnB Clone Backend Requirements

## 1. User Authentication System

### 1.1 Functional Requirements

- User registration (guest/host roles)  
- User login with JWT authentication  
- Password reset functionality  
- Profile management  

### 1.2 API Endpoints

**Registration**  
`POST /api/v1/auth/register`  
**Request Body:**  
- `first_name`, `last_name`, `email`, `password`, `phone_number`, `role`  
**Response:**  
- `user_id`, user details, confirmation message  

**Login**  
`POST /api/v1/auth/login`  
**Request Body:**  
- `email`, `password`  
**Response:**  
- `access_token`, user details  

**Profile Management**  
- `GET /api/v1/users/profile`  
- `PUT /api/v1/users/profile`  
**Request Body (PUT):**  
- `first_name`, `last_name`, `phone_number`, `profile_image`  
**Response:**  
- Updated user details  

### 1.3 Validation Rules

- **Email:** Valid format, unique in system  
- **Password:** Min 8 chars, require uppercase, lowercase, number, special char  
- **Names:** 2–50 chars, alphabetic  

### 1.4 Security Requirements

- Password hashing with bcrypt  
- JWT token expiration after 24 hours  
- Rate limiting on failed login attempts  
- HTTPS for all endpoints  

### 1.5 Performance Criteria

- Registration: < 3 seconds  
- Login: < 2 seconds  
- Profile operations: < 2 seconds  
- Handle 100+ concurrent authentication requests  

---

## 2. Property Management System

### 2.1 Functional Requirements

- Property creation with multiple images  
- Property listing management (update, delete)  
- Availability and pricing settings  
- Property search and filtering  

### 2.2 API Endpoints

**Create Property**  
`POST /api/v1/properties`  
**Request Body:**  
- `name`, `description`, `location`, `price`, `property details`, `amenities`, `images`  
**Response:**  
- `property_id`, property details  

**Get/Update Property**  
- `GET /api/v1/properties/{property_id}`  
- `PUT /api/v1/properties/{property_id}`  
**Request Body (PUT):**  
- Any property fields to update  
**Response:**  
- Updated property details  

**Search Properties**  
`GET /api/v1/properties/search?params...`  
**Query Params:**  
- `location`, `dates`, `guests`, `price range`, `page`, `limit`  
**Response:**  
- Paginated property listings  

### 2.3 Validation Rules

- **Name:** 5–100 chars  
- **Description:** 20–2000 chars  
- **Price:** > 0, numeric  
- **Images:** 1–10 images, max 5MB each, JPG/PNG formats  

### 2.4 Security Requirements

- Only hosts can create/edit their own properties  
- Image validation and scanning  
- Location verification  

### 2.5 Performance Criteria

- Property creation: < 5 seconds  
- Search results: < 2 seconds  
- Handle 1000+ search requests per minute  

---

## 3. Booking System

### 3.1 Functional Requirements

- Property booking for specific dates  
- Availability verification  
- Payment processing  
- Booking management (view, cancel)  
- Review submission after stays  

### 3.2 API Endpoints

**Create Booking**  
`POST /api/v1/bookings`  
**Request Body:**  
- `property_id`, `check_in_date`, `check_out_date`, `guests_count`, `payment_method_id`  
**Response:**  
- `booking_id`, booking details, confirmation  

**Get/Cancel Booking**  
- `GET /api/v1/bookings/{booking_id}`  
- `PUT /api/v1/bookings/{booking_id}/cancel`  
**Response:**  
- Booking details, refund info (for cancellation)  

**Submit Review**  
`POST /api/v1/properties/{property_id}/reviews`  
**Request Body:**  
- `booking_id`, `rating`, `comment`  
**Response:**  
- `review_id`, review details  

### 3.3 Validation Rules

- **Check-in date:** Future date  
- **Check-out date:** After check-in date  
- **Guests count:** ≤ property `max_guests`  
- **Rating:** 1–5 integer  
- **Review comment:** 10–500 chars  

### 3.4 Security Requirements

- Users can only view/manage their own bookings  
- Hosts can only view bookings for their properties  
- Secure payment processing (PCI-DSS compliance)  

### 3.5 Performance Criteria

- Availability check: < 1 second  
- Booking creation: < 5 seconds  
- Handle 50+ concurrent booking requests  

---

## 4. General Requirements

### 4.1 Technical Stack

- Python 3.8+  
- Flask/Django  
- PostgreSQL  
- JWT Authentication  

### 4.2 API Standards

- RESTful design  
- Versioned endpoints (`/api/v1/...`)  
- Consistent response format  
- Proper HTTP status codes  

### 4.3 Error Handling

```json
{
  "status": "error",
  "code": "error_code",
  "message": "Human-readable error message"
}
