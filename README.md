# Travel Booking Platform

This project implements a **Travel Booking Platform** using a microservices architecture. It includes a backend with three services (Booking, Payment, and Notification), a PostgreSQL database, and a web-based frontend. All components are containerized using Docker, with orchestration managed by Docker Compose.


## **Project Directory Structure**

```
travel-booking-platform/
├── backend/
│   ├── booking-service/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   ├── payment-service/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   └── notification-service/
│       ├── app.py
│       ├── requirements.txt
│       └── Dockerfile
├── db/
│   ├── init.sql
│   └── Dockerfile
├── frontend/
│   ├── index.html
│   ├── styles.css
│   ├── app.js
│   └── Dockerfile
├── docker-compose.yml

```

---

## **Setup Guide**

### **1. Prerequisites**
- Docker installed on your machine.
- Docker Compose installed.

### **2. Build and Run Services**
To build and run all the services, execute the following command:
```bash
docker-compose up --build
```
This will:
- Build and run each microservice (Booking, Payment, and Notification).
- Set up the PostgreSQL database.
- Serve the frontend via an NGINX container.

### **3. Access the Application**
- Frontend: [http://localhost:8080](http://localhost:8080)
- Booking API: [http://localhost:5001](http://localhost:5001)
- Payment API: [http://localhost:5002](http://localhost:5002)
- Notification API: [http://localhost:5003](http://localhost:5003)

---

## **Components**

### **1. Backend - Microservices**
#### **1.1 Booking Service**
- **File:** `backend/booking-service/app.py`
- **Purpose:** Handles booking requests and interacts with the Payment and Notification services.
- **API Endpoint:**
  - `/book` (POST): Processes booking by interacting with Payment and Notification services.
- **Dependencies:** Flask, requests

#### **1.2 Payment Service**
- **File:** `backend/payment-service/app.py`
- **Purpose:** Handles payment processing.
- **API Endpoint:**
  - `/pay` (POST): Processes payment requests.
- **Dependencies:** Flask

#### **1.3 Notification Service**
- **File:** `backend/notification-service/app.py`
- **Purpose:** Sends notifications.
- **API Endpoint:**
  - `/notify` (POST): Sends a notification message.
- **Dependencies:** Flask

---

### **2. Database**
#### **Database Setup**
- **File:** `db/init.sql`
- **Purpose:** Initializes the database schema.
- **Dockerfile Configuration:**
  - Sets up PostgreSQL with the initial schema.
---

### **3. Frontend**
#### **File:** `frontend/index.html`
- **Purpose:** Provides a user interface for booking interactions.
- **Interactions:**
  - Collects booking amount from the user.
  - Sends data to the Booking API.

#### **Supporting Files:**
- `styles.css`: Styles the web interface.
- `app.js`: Contains JavaScript for handling form submissions and API communication.

---

## **Docker Compose Configuration**
### **File:** `docker-compose.yml`

```yaml
version: '3.9'
services:
  booking-service:
    build: ./backend/booking-service
    ports:
      - "5001:5000"
    depends_on:
      - payment-service
      - notification-service

  payment-service:
    build: ./backend/payment-service
    ports:
      - "5002:5000"

  notification-service:
    build: ./backend/notification-service
    ports:
      - "5003:5000"

  database:
    build: ./db
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: password
      POSTGRES_DB: travel
    ports:
      - "5432:5432"

  frontend:
    build: ./frontend
    ports:
      - "8080:80"
```

## **Interservice Communication**
1. **Booking Service:**
   - Calls Payment API (`/pay`) to process payments.
   - Calls Notification API (`/notify`) to send booking confirmation messages.

2. **Payment Service:**
   - Receives payment requests from the Booking Service.
   - Responds with payment status and amount.

3. **Notification Service:**
   - Receives notification requests from the Booking Service.
   - Responds with notification status and message.

---

## **Deployment Steps**


1. **Run the Containers:**
   ```bash
   docker-compose up
   ```

2. **Stop the Services:**
   ```bash
   docker-compose down
   ```

---



