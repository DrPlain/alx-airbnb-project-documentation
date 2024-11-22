
# **Backend Requirements Specification**

## **1. User Authentication and Management**

### **Objective**
Enable secure and seamless user registration, login, and profile management for guests and hosts.

---

### **Functional Requirements**
- Allow users to register accounts as guests or hosts.
- Enable secure login using email and password.
- Support OAuth authentication (e.g., Google, Facebook).
- Allow users to update their profiles.

---

### **API Endpoints**
1. **Register User**
   - **Endpoint**: `POST /api/auth/register`
   - **Input**:  
     ```json
     {
       "email": "user@example.com",
       "password": "password123",
       "role": "guest/host"
     }
     ```
   - **Output**:  
     ```json
     {
       "message": "Registration successful",
       "userId": "12345"
     }
     ```

2. **Login User**
   - **Endpoint**: `POST /api/auth/login`
   - **Input**:  
     ```json
     {
       "email": "user@example.com",
       "password": "password123"
     }
     ```
   - **Output**:  
     ```json
     {
       "token": "jwt_token",
       "userId": "12345"
     }
     ```

3. **Update Profile**
   - **Endpoint**: `PUT /api/user/profile`
   - **Input**:  
     ```json
     {
       "userId": "12345",
       "profilePhoto": "profile_photo_url",
       "contactInfo": "555-123-456",
       "preferences": {
         "currency": "USD",
         "language": "en-US"
       }
     }
     ```
   - **Output**:  
     ```json
     {
       "message": "Profile updated successfully"
     }
     ```

---

### **Validation Rules**
- Email must be unique and in a valid format.
- Password must be at least 8 characters, including one number and one special character.
- Role must be either "guest" or "host."

---

### **Performance Criteria**
- Authentication requests should be processed within 300ms.
- Password encryption must use a minimum of 12 rounds of hashing (e.g., bcrypt).

---

## **2. Property Management**

### **Objective**
Provide hosts with the ability to add, update, and remove property listings.

---

### **Functional Requirements**
- Allow hosts to add property details, including title, description, amenities, and availability.
- Enable hosts to edit or delete property listings.

---

### **API Endpoints**
1. **Add Property Listing**
   - **Endpoint**: `POST /api/properties`
   - **Input**:  
     ```json
     {
       "hostId": "12345",
       "title": "Cozy Apartment in Downtown",
       "description": "A comfortable 2-bedroom apartment close to major attractions.",
       "location": "New York, NY",
       "pricePerNight": 150,
       "amenities": ["Wi-Fi", "Kitchen", "Parking"],
       "availability": ["2024-11-01", "2024-11-30"]
     }
     ```
   - **Output**:  
     ```json
     {
       "message": "Property added successfully",
       "propertyId": "67890"
     }
     ```

2. **Edit Property Listing**
   - **Endpoint**: `PUT /api/properties/:propertyId`
   - **Input**:  
     ```json
     {
       "title": "Updated Cozy Apartment in Downtown",
       "pricePerNight": 175
     }
     ```
   - **Output**:  
     ```json
     {
       "message": "Property updated successfully"
     }
     ```

3. **Delete Property Listing**
   - **Endpoint**: `DELETE /api/properties/:propertyId`
   - **Output**:  
     ```json
     {
       "message": "Property deleted successfully"
     }
     ```

---

### **Validation Rules**
- Property title must not exceed 100 characters.
- Price per night must be a positive number.
- Availability dates must be in the future.

---

### **Performance Criteria**
- Property creation and updates should complete within 400ms.
- Pagination for property listings should handle up to 10,000 properties efficiently.

---

## **3. Booking System**

### **Objective**
Allow guests to book properties, manage booking statuses, and prevent double bookings.

---

### **Functional Requirements**
- Enable guests to book properties for specific dates.
- Prevent bookings if the property is unavailable.
- Allow cancellation of bookings based on the host’s policy.
- Track booking statuses: Pending, Confirmed, Canceled, Completed.

---

### **API Endpoints**
1. **Create Booking**
   - **Endpoint**: `POST /api/bookings`
   - **Input**:  
     ```json
     {
       "guestId": "54321",
       "propertyId": "67890",
       "checkInDate": "2024-11-10",
       "checkOutDate": "2024-11-15"
     }
     ```
   - **Output**:  
     ```json
     {
       "message": "Booking created successfully",
       "bookingId": "11223"
     }
     ```

2. **Cancel Booking**
   - **Endpoint**: `PUT /api/bookings/:bookingId/cancel`
   - **Output**:  
     ```json
     {
       "message": "Booking canceled successfully"
     }
     ```

3. **View Booking Status**
   - **Endpoint**: `GET /api/bookings/:bookingId`
   - **Output**:  
     ```json
     {
       "bookingId": "11223",
       "status": "Confirmed",
       "checkInDate": "2024-11-10",
       "checkOutDate": "2024-11-15"
     }
     ```

---

### **Validation Rules**
- Check-in date must not be earlier than the current date.
- Check-out date must be after the check-in date.
- Booking must not overlap with existing reservations.

---

### **Performance Criteria**
- Booking validations should complete within 300ms.
- System should handle up to 1,000 concurrent booking requests.
