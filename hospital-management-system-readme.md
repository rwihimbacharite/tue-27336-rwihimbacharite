# Hospital Management System - Database Implementation

## Student Information
- Student ID: 27336
- Name: Your Migisha Rwihimba Charite
- Concentration: Software Engineering

## Phase 1: Problem Statement

### Problem Definition
The Hospital Management System (HMS) is designed to streamline and automate healthcare operations of a hospital facility. It provides a comprehensive database solution to manage patient information, physician details, appointments scheduling, medical records, prescription tracking, and inventory management.

### Context
This system will be implemented in mid-sized to large hospital facilities where manual record-keeping has become inefficient and error-prone. The HMS will serve as the central data management system connecting all hospital departments.

### Target Users
- Hospital administrative staff
- Doctors and medical professionals
- Nurses and support staff
- Pharmacy department
- Patients (for appointment viewing/scheduling)

### Project Goals
- Streamline patient registration and information management
- Automate appointment scheduling and tracking
- Maintain comprehensive electronic medical records
- Track medication prescriptions and inventory
- Enhance data security and access control
- Generate reports for administrative decision-making
- Reduce paperwork and administrative overhead
- Minimize errors in patient care and medication administration

## Phase 2: Business Process Modeling

### Appointment Scheduling Process

![Business Process Model](images/appointment_process_bpmn.png)

The appointment scheduling process involves multiple actors and systems within the hospital environment. The diagram uses BPMN notation to illustrate the flow of information and activities among different departments.

#### Main Components and Interactions:
1. **Patient** initiates the process by requesting an appointment either in person, by phone, or through an online portal
2. **Receptionist** checks doctor availability in the system
3. **System** verifies patient records and insurance information
4. **Doctor** receives scheduled appointments and can approve or request rescheduling
5. **Notifications** are sent to patients confirming their appointments

#### MIS Support:
This process supports MIS functions by:
- Improving decision-making through real-time availability information
- Streamlining operations by automating previously manual scheduling tasks
- Reducing scheduling conflicts and optimizing resource allocation
- Providing data for analytics on appointment patterns and resource utilization

#### Organizational Importance:
Efficient appointment scheduling is critical for hospital operations as it directly impacts patient satisfaction, resource utilization, and revenue generation. The automated system reduces administrative overhead while ensuring optimal utilization of doctor availability.

## Phase 3: Logical Model Design

### Entity-Relationship (ER) Model

![Hospital Management System ER Diagram](images/hospital_er_diagram.png)

### Entities and Attributes

1. **Departments**
   - department_id (PK): NUMBER
   - department_name: VARCHAR2(100)
   - location: VARCHAR2(100)
   - head_doctor_id (FK): NUMBER
   - contact_number: VARCHAR2(20)
   - created_date: TIMESTAMP

2. **Doctors**
   - doctor_id (PK): NUMBER
   - first_name: VARCHAR2(50) NOT NULL
   - last_name: VARCHAR2(50) NOT NULL
   - specialization: VARCHAR2(100)
   - department_id (FK): NUMBER
   - contact_number: VARCHAR2(20)
   - email: VARCHAR2(100)
   - hire_date: DATE
   - created_date: TIMESTAMP

3. **Patients**
   - patient_id (PK): NUMBER
   - first_name: VARCHAR2(50) NOT NULL
   - last_name: VARCHAR2(50) NOT NULL
   - date_of_birth: DATE NOT NULL
   - gender: VARCHAR2(10)
   - blood_group: VARCHAR2(5)
   - address: VARCHAR2(200)
   - contact_number: VARCHAR2(20)
   - email: VARCHAR2(100)
   - emergency_contact: VARCHAR2(100)
   - insurance_details: VARCHAR2(200)
   - registration_date: DATE
   - created_date: TIMESTAMP

4. **Appointments**
   - appointment_id (PK): NUMBER
   - patient_id (FK): NUMBER
   - doctor_id (FK): NUMBER
   - appointment_date: DATE NOT NULL
   - appointment_time: TIMESTAMP NOT NULL
   - purpose: VARCHAR2(200)
   - status: VARCHAR2(20) CHECK (status IN ('Scheduled', 'Completed', 'Cancelled', 'No-Show'))
   - notes: VARCHAR2(500)
   - created_date: TIMESTAMP

5. **Medical_Records**
   - record_id (PK): NUMBER
   - patient_id (FK): NUMBER
   - doctor_id (FK): NUMBER
   - diagnosis: VARCHAR2(200)
   - treatment: VARCHAR2(500)
   - prescription: VARCHAR2(500)
   - notes: VARCHAR2(1000)
   - record_date: DATE
   - follow_up_date: DATE
   - created_date: TIMESTAMP

6. **Medications**
   - medication_id (PK): NUMBER
   - medication_name: VARCHAR2(100) NOT NULL
   - description: VARCHAR2(500)
   - dosage_form: VARCHAR2(50)
   - manufacturer: VARCHAR2(100)
   - unit_price: NUMBER(10,2)
   - stock_quantity: NUMBER
   - created_date: TIMESTAMP

7. **Prescriptions**
   - prescription_id (PK): NUMBER
   - record_id (FK): NUMBER
   - medication_id (FK): NUMBER
   - dosage: VARCHAR2(50)
   - frequency: VARCHAR2(50)
   - duration: VARCHAR2(50)
   - notes: VARCHAR2(200)
   - created_date: TIMESTAMP

8. **Public_Holidays** (Added for Phase 7 requirements)
   - holiday_id (PK): NUMBER
   - holiday_date: DATE NOT NULL
   - holiday_name: VARCHAR2(100)
   - description: VARCHAR2(200)

9. **Audit_Logs** (Added for Phase 7 requirements)
   - log_id (PK): NUMBER
   - user_id: VARCHAR2(50)
   - action_date: TIMESTAMP
   - table_name: VARCHAR2(50)
   - operation: VARCHAR2(20)
   - status: VARCHAR2(20)
   - details: VARCHAR2(500)

### Relationships & Constraints

- **Doctors to Departments**: Many-to-One (Doctors belong to one department)
- **Departments to Doctors**: One-to-One (Each department has one head doctor)
- **Patients to Appointments**: One-to-Many (Patients can have multiple appointments)
- **Doctors to Appointments**: One-to-Many (Doctors can have multiple appointments)
- **Patients to Medical_Records**: One-to-Many (Patients can have multiple medical records)
- **Doctors to Medical_Records**: One-to-Many (Doctors can create multiple medical records)
- **Medical_Records to Medications**: Many-to-Many (through Prescriptions junction table)

### Normalization

The database design adheres to the Third Normal Form (3NF):
- All non-key attributes are fully dependent on the primary key
- No transitive dependencies
- No partial dependencies on composite keys
- Separate tables for independent entities

## Phase 4: Database Creation and Naming

### Database Creation

```sql
-- Create Pluggable Database
CREATE PLUGGABLE DATABASE tue_27336_rwihimbacharite_hospitalMS_db
ADMIN USER admin IDENTIFIED BY Rwihimba1
FILE_NAME_CONVERT = ('/u01/app/oracle/oradata/ORCL/pdbseed/',
                     '/u01/app/oracle/oradata/ORCL/tue_27336_rwihimbacharite_hospitalMS_db/');

-- Open the PDB
ALTER PLUGGABLE DATABASE tue_27336_rwihimbacharite_hospitalMS_db OPEN;

-- Switch to the PDB
ALTER SESSION SET CONTAINER = tue_27336_rwihimbacharite_hospitalMS_db;

-- Grant necessary privileges to the admin user
GRANT CREATE SESSION, CREATE TABLE, CREATE PROCEDURE, CREATE SEQUENCE, 
      CREATE TRIGGER, CREATE VIEW, CREATE MATERIALIZED VIEW, 
      CREATE DATABASE LINK, CREATE SYNONYM, CREATE TYPE 
      TO admin;
```

### Oracle Enterprise Manager (OEM) Configuration

![OEM Dashboard Screenshot](images/oem_dashboard.png)

The Oracle Enterprise Manager has been configured to monitor the newly created database. It provides comprehensive monitoring capabilities including:
- Performance monitoring
- Storage usage
- User session tracking
- Alert notifications
- Backup status

## Phase 5: Table Implementation and Data Insertion

### Table Creation Scripts

```sql
-- Create Sequences for Auto-Incrementing IDs
CREATE SEQUENCE dept_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE doctor_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE patient_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE appt_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE record_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE med_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE presc_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE holiday_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE audit_seq START WITH 1 INCREMENT BY 1;

-- Create Departments Table
CREATE TABLE Departments (
    department_id NUMBER PRIMARY KEY,
    department_name VARCHAR2(100) NOT NULL,
    location VARCHAR2(100),
    head_doctor_id NUMBER,
    contact_number VARCHAR2(20),
    created_date TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Create Doctors Table
CREATE TABLE Doctors (
    doctor_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50) NOT NULL,
    last_name VARCHAR2(50) NOT NULL,
    specialization VARCHAR2(100),
    department_id NUMBER,
    contact_number VARCHAR2(20),
    email VARCHAR2(100),
    hire_date DATE,
    created_date TIMESTAMP DEFAULT SYSTIMESTAMP,
    CONSTRAINT fk_doctor_dept FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);

-- Add Foreign Key for head_doctor_id in Departments
ALTER TABLE Departments
ADD CONSTRAINT fk_dept_head_doctor FOREIGN KEY (head_doctor_id) REFERENCES Doctors(doctor_id);

-- Create Patients Table
CREATE TABLE Patients (
    patient_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50) NOT NULL,
    last_name VARCHAR2(50) NOT NULL,
    date_of_birth DATE NOT NULL,
    gender VARCHAR2(10),
    blood_group VARCHAR2(5),
    address VARCHAR2(200),
    contact_number VARCHAR2(20),
    email VARCHAR2(100),
    emergency_contact VARCHAR2(100),
    insurance_details VARCHAR2(200),
    registration_date DATE DEFAULT SYSDATE,
    created_date TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Create Appointments Table
CREATE TABLE Appointments (
    appointment_id NUMBER PRIMARY KEY,
    patient_id NUMBER,
    doctor_id NUMBER,
    appointment_date DATE NOT NULL,
    appointment_time TIMESTAMP NOT NULL,
    purpose VARCHAR2(200),
    status VARCHAR2(20) CHECK (status IN ('Scheduled', 'Completed', 'Cancelled', 'No-Show')),
    notes VARCHAR2(500),
    created_date TIMESTAMP DEFAULT SYSTIMESTAMP,
    CONSTRAINT fk_appt_patient FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    CONSTRAINT fk_appt_doctor FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id)
);

-- Create Medical_Records Table
CREATE TABLE Medical_Records (
    record_id NUMBER PRIMARY KEY,
    patient_id NUMBER,
    doctor_id NUMBER,
    diagnosis VARCHAR2(200),
    treatment VARCHAR2(500),
    prescription VARCHAR2(500),
    notes VARCHAR2(1000),
    record_date DATE DEFAULT SYSDATE,
    follow_up_date DATE,
    created_date TIMESTAMP DEFAULT SYSTIMESTAMP,
    CONSTRAINT fk_record_patient FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    CONSTRAINT fk_record_doctor FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id)
);

-- Create Medications Table
CREATE TABLE Medications (
    medication_id NUMBER PRIMARY KEY,
    medication_name VARCHAR2(100) NOT NULL,
    description VARCHAR2(500),
    dosage_form VARCHAR2(50),
    manufacturer VARCHAR2(100),
    unit_price NUMBER(10,2),
    stock_quantity NUMBER,
    created_date TIMESTAMP DEFAULT SYSTIMESTAMP
);

-- Create Prescriptions Junction Table
CREATE TABLE Prescriptions (
    prescription_id NUMBER PRIMARY KEY,
    record_id NUMBER,
    medication_id NUMBER,
    dosage VARCHAR2(50),
    frequency VARCHAR2(50),
    duration VARCHAR2(50),
    notes VARCHAR2(200),
    created_date TIMESTAMP DEFAULT SYSTIMESTAMP,
    CONSTRAINT fk_presc_record FOREIGN KEY (record_id) REFERENCES Medical_Records(record_id),
    CONSTRAINT fk_presc_medication FOREIGN KEY (medication_id) REFERENCES Medications(medication_id)
);

-- Create Public_Holidays Table
CREATE TABLE Public_Holidays (
    holiday_id NUMBER PRIMARY KEY,
    holiday_date DATE NOT NULL,
    holiday_name VARCHAR2(100),
    description VARCHAR2(200)
);

-- Create Audit_Logs Table
CREATE TABLE Audit_Logs (
    log_id NUMBER PRIMARY KEY,
    user_id VARCHAR2(50),
    action_date TIMESTAMP DEFAULT SYSTIMESTAMP,
    table_name VARCHAR2(50),
    operation VARCHAR2(20),
    status VARCHAR2(20),
    details VARCHAR2(500)
);
```

### Data Insertion

Sample data has been inserted into all tables to simulate a functioning hospital environment. Below are examples of the insertion scripts used:

```sql
-- Insert Departments
INSERT INTO Departments (department_id, department_name, location, contact_number)
VALUES (dept_seq.NEXTVAL, 'Cardiology', 'Building A, Floor 3', '+1234567890');

INSERT INTO Departments (department_id, department_name, location, contact_number)
VALUES (dept_seq.NEXTVAL, 'Neurology', 'Building B, Floor 2', '+1234567891');

-- Insert Doctors
INSERT INTO Doctors (doctor_id, first_name, last_name, specialization, department_id, contact_number, email, hire_date)
VALUES (doctor_seq.NEXTVAL, 'John', 'Smith', 'Cardiologist', 1, '+1234567892', 'john.smith@hospital.com', TO_DATE('15-Jan-2020', 'DD-MON-YYYY'));

INSERT INTO Doctors (doctor_id, first_name, last_name, specialization, department_id, contact_number, email, hire_date)
VALUES (doctor_seq.NEXTVAL, 'Sarah', 'Johnson', 'Neurologist', 2, '+1234567893', 'sarah.johnson@hospital.com', TO_DATE('10-Mar-2019', 'DD-MON-YYYY'));

-- Update Department Heads
UPDATE Departments SET head_doctor_id = 1 WHERE department_id = 1;
UPDATE Departments SET head_doctor_id = 2 WHERE department_id = 2;

-- Insert Patients
INSERT INTO Patients (patient_id, first_name, last_name, date_of_birth, gender, blood_group, address, contact_number, email, emergency_contact, insurance_details)
VALUES (patient_seq.NEXTVAL, 'Michael', 'Brown', TO_DATE('25-Apr-1985', 'DD-MON-YYYY'), 'Male', 'O+', '123 Main St, Cityville', '+1234567894', 'michael.brown@email.com', '+1234567895', 'BlueShield #12345678');

-- Insert Public Holidays for upcoming month
INSERT INTO Public_Holidays (holiday_id, holiday_date, holiday_name, description)
VALUES (holiday_seq.NEXTVAL, TO_DATE('01-May-2025', 'DD-MON-YYYY'), 'Labor Day', 'International Workers Day');

INSERT INTO Public_Holidays (holiday_id, holiday_date, holiday_name, description)
VALUES (holiday_seq.NEXTVAL, TO_DATE('15-May-2025', 'DD-MON-YYYY'), 'Healthcare Professionals Day', 'Day honoring healthcare workers');
```

Additional data was inserted to ensure comprehensive testing and demonstration of all system features. In total, the database contains:
- 8 departments
- 25 doctors
- 100 patients
- 250 appointments
- 180 medical records
- 50 medications
- 300 prescriptions
- 6 public holidays for the upcoming month

## Phase 6: Database Interaction and Transactions

### Procedures, Functions, and Packages

#### Package: Patient Management

```sql
CREATE OR REPLACE PACKAGE patient_mgmt AS
    -- Procedure to register a new patient
    PROCEDURE register_patient(
        p_first_name IN VARCHAR2,
        p_last_name IN VARCHAR2,
        p_dob IN DATE,
        p_gender IN VARCHAR2,
        p_blood_group IN VARCHAR2,
        p_address IN VARCHAR2,
        p_contact IN VARCHAR2,
        p_email IN VARCHAR2,
        p_emergency IN VARCHAR2,
        p_insurance IN VARCHAR2,
        p_patient_id OUT NUMBER
    );
    
    -- Function to check if patient exists
    FUNCTION patient_exists(p_patient_id IN NUMBER) RETURN BOOLEAN;
    
    -- Procedure to schedule appointment
    PROCEDURE schedule_appointment(
        p_patient_id IN NUMBER,
        p_doctor_id IN NUMBER,
        p_appt_date IN DATE,
        p_appt_time IN TIMESTAMP,
        p_purpose IN VARCHAR2,
        p_appointment_id OUT NUMBER
    );
    
    -- Function to get patient appointment count
    FUNCTION get_appointment_count(p_patient_id IN NUMBER) RETURN NUMBER;
END patient_mgmt;
/

CREATE OR REPLACE PACKAGE BODY patient_mgmt AS
    -- Procedure to register a new patient
    PROCEDURE register_patient(
        p_first_name IN VARCHAR2,
        p_last_name IN VARCHAR2,
        p_dob IN DATE,
        p_gender IN VARCHAR2,
        p_blood_group IN VARCHAR2,
        p_address IN VARCHAR2,
        p_contact IN VARCHAR2,
        p_email IN VARCHAR2,
        p_emergency IN VARCHAR2,
        p_insurance IN VARCHAR2,
        p_patient_id OUT NUMBER
    ) IS
    BEGIN
        -- Insert new patient record
        INSERT INTO Patients (
            patient_id, first_name, last_name, date_of_birth, gender, 
            blood_group, address, contact_number, email, 
            emergency_contact, insurance_details
        ) VALUES (
            patient_seq.NEXTVAL, p_first_name, p_last_name, p_dob, p_gender,
            p_blood_group, p_address, p_contact, p_email,
            p_emergency, p_insurance
        ) RETURNING patient_id INTO p_patient_id;
        
        COMMIT;
    EXCEPTION
        WHEN OTHERS THEN
            ROLLBACK;
            RAISE;
    END register_patient;
    
    -- Function to check if patient exists
    FUNCTION patient_exists(p_patient_id IN NUMBER) RETURN BOOLEAN IS
        v_count NUMBER;
    BEGIN
        SELECT COUNT(*) INTO v_count
        FROM Patients
        WHERE patient_id = p_patient_id;
        
        RETURN (v_count > 0);
    END patient_exists;
    
    -- Procedure to schedule appointment
    PROCEDURE schedule_appointment(
        p_patient_id IN NUMBER,
        p_doctor_id IN NUMBER,
        p_appt_date IN DATE,
        p_appt_time IN TIMESTAMP,
        p_purpose IN VARCHAR2,
        p_appointment_id OUT NUMBER
    ) IS
        v_patient_exists BOOLEAN;
        v_doctor_exists BOOLEAN;
        v_patient_count NUMBER;
        v_doctor_count NUMBER;
    BEGIN
        -- Check if patient and doctor exist
        SELECT COUNT(*) INTO v_patient_count FROM Patients WHERE patient_id = p_patient_id;
        SELECT COUNT(*) INTO v_doctor_count FROM Doctors WHERE doctor_id = p_doctor_id;
        
        v_patient_exists := (v_patient_count > 0);
        v_doctor_exists := (v_doctor_count > 0);
        
        IF NOT v_patient_exists THEN
            RAISE_APPLICATION_ERROR(-20001, 'Patient does not exist');
        END IF;
        
        IF NOT v_doctor_exists THEN
            RAISE_APPLICATION_ERROR(-20002, 'Doctor does not exist');
        END IF;
        
        -- Insert new appointment
        INSERT INTO Appointments (
            appointment_id, patient_id, doctor_id, appointment_date,
            appointment_time, purpose, status
        ) VALUES (
            appt_seq.NEXTVAL, p_patient_id, p_doctor_id, p_appt_date,
            p_appt_time, p_purpose, 'Scheduled'
        ) RETURNING appointment_id INTO p_appointment_id;
        
        COMMIT;
    EXCEPTION
        WHEN OTHERS THEN
            ROLLBACK;
            RAISE;
    END schedule_appointment;
    
    -- Function to get patient appointment count
    FUNCTION get_appointment_count(p_patient_id IN NUMBER) RETURN NUMBER IS
        v_count NUMBER;
    BEGIN
        SELECT COUNT(*) INTO v_count
        FROM Appointments
        WHERE patient_id = p_patient_id;
        
        RETURN v_count;
    END get_appointment_count;
END patient_mgmt;
/
```

#### Package: Medical Records Management

```sql
CREATE OR REPLACE PACKAGE med_records_mgmt AS
    -- Procedure to create a new medical record
    PROCEDURE create_medical_record(
        p_patient_id IN NUMBER,
        p_doctor_id IN NUMBER,
        p_diagnosis IN VARCHAR2,
        p_treatment IN VARCHAR2,
        p_notes IN VARCHAR2,
        p_follow_up_date IN DATE,
        p_record_id OUT NUMBER
    );
    
    -- Procedure to add prescription to record
    PROCEDURE add_prescription(
        p_record_id IN NUMBER,
        p_medication_id IN NUMBER,
        p_dosage IN VARCHAR2,
        p_frequency IN VARCHAR2,
        p_duration IN VARCHAR2,
        p_notes IN VARCHAR2,
        p_prescription_id OUT NUMBER
    );
    
    -- Function to get patient medical record count
    FUNCTION get_record_count(p_patient_id IN NUMBER) RETURN NUMBER;
    
    -- Procedure to get patient recent diagnoses using cursor
    PROCEDURE get_recent_diagnoses(
        p_patient_id IN NUMBER,
        p_days IN NUMBER DEFAULT 30
    );
END med_records_mgmt;
/

CREATE OR REPLACE PACKAGE BODY med_records_mgmt AS
    -- Procedure to create a new medical record
    PROCEDURE create_medical_record(
        p_patient_id IN NUMBER,
        p_doctor_id IN NUMBER,
        p_diagnosis IN VARCHAR2,
        p_treatment IN VARCHAR2,
        p_notes IN VARCHAR2,
        p_follow_up_date IN DATE,
        p_record_id OUT NUMBER
    ) IS
    BEGIN
        -- Insert new medical record
        INSERT INTO Medical_Records (
            record_id, patient_id, doctor_id, diagnosis,
            treatment, notes, record_date, follow_up_date
        ) VALUES (
            record_seq.NEXTVAL, p_patient_id, p_doctor_id, p_diagnosis,
            p_treatment, p_notes, SYSDATE, p_follow_up_date
        ) RETURNING record_id INTO p_record_id;
        
        COMMIT;
    EXCEPTION
        WHEN OTHERS THEN
            ROLLBACK;
            RAISE;
    END create_medical_record;
    
    -- Procedure to add prescription to record
    PROCEDURE add_prescription(
        p_record_id IN NUMBER,
        p_medication_id IN NUMBER,
        p_dosage IN VARCHAR2,
        p_frequency IN VARCHAR2,
        p_duration IN VARCHAR2,
        p_notes IN VARCHAR2,
        p_prescription_id OUT NUMBER
    ) IS
        v_med_exists NUMBER;
        v_record_exists NUMBER;
        v_current_stock NUMBER;
    BEGIN
        -- Check if medication and record exist
        SELECT COUNT(*) INTO v_med_exists FROM Medications WHERE medication_id = p_medication_id;
        SELECT COUNT(*) INTO v_record_exists FROM Medical_Records WHERE record_id = p_record_id;
        
        IF v_med_exists = 0 THEN
            RAISE_APPLICATION_ERROR(-20003, 'Medication does not exist');
        END IF;
        
        IF v_record_exists = 0 THEN
            RAISE_APPLICATION_ERROR(-20004, 'Medical record does not exist');
        END IF;
        
        -- Check medication stock
        SELECT stock_quantity INTO v_current_stock
        FROM Medications
        WHERE medication_id = p_medication_id;
        
        IF v_current_stock <= 0 THEN
            RAISE_APPLICATION_ERROR(-20005, 'Medication out of stock');
        END IF;
        
        -- Insert new prescription
        INSERT INTO Prescriptions (
            prescription_id, record_id, medication_id, dosage,
            frequency, duration, notes
        ) VALUES (
            presc_seq.NEXTVAL, p_record_id, p_medication_id, p_dosage,
            p_frequency, p_duration, p_notes
        ) RETURNING prescription_id INTO p_prescription_id;
        
        -- Update medication stock
        UPDATE Medications
        SET stock_quantity = stock_quantity - 1
        WHERE medication_id = p_medication_id;
        
        COMMIT;
    EXCEPTION
        WHEN OTHERS THEN
            ROLLBACK;
            RAISE;
    END add_prescription;
    
    -- Function to get patient medical record count
    FUNCTION get_record_count(p_patient_id IN NUMBER) RETURN NUMBER IS
        v_count NUMBER;
    BEGIN
        SELECT COUNT(*) INTO v_count
        FROM Medical_Records
        WHERE patient_id = p_patient_id;
        
        RETURN v_count;
    END get_record_count;
    
    -- Procedure to get patient recent diagnoses using cursor
    PROCEDURE get_recent_diagnoses(
        p_patient_id IN NUMBER,
        p_days IN NUMBER DEFAULT 30
    ) IS
        CURSOR c_diagnoses IS
            SELECT mr.diagnosis, mr.record_date,
                   d.first_name || ' ' || d.last_name AS doctor_name
            FROM Medical_Records mr
            JOIN Doctors d ON mr.doctor_id = d.doctor_id
            WHERE mr.patient_id = p_patient_id
            AND mr.record_date >= SYSDATE - p_days
            ORDER BY mr.record_date DESC;
            
        v_diagnosis c_diagnoses%ROWTYPE;
        v_patient_name VARCHAR2(100);
    BEGIN
        -- Get patient name
        SELECT first_name || ' ' || last_name INTO v_patient_name
        FROM Patients
        WHERE patient_id = p_patient_id;
        
        DBMS_OUTPUT.PUT_LINE('Recent diagnoses for patient: ' || v_patient_name);
        DBMS_OUTPUT.PUT_LINE('----------------------------------------');
        
        OPEN c_diagnoses;
        LOOP
            FETCH c_diagnoses INTO v_diagnosis;
            EXIT WHEN c_diagnoses%NOTFOUND;
            
            DBMS_OUTPUT.PUT_LINE('Date: ' || TO_CHAR(v_diagnosis.record_date, 'DD-MON-YYYY'));
            DBMS_OUTPUT.PUT_LINE('Diagnosis: ' || v_diagnosis.diagnosis);
            DBMS_OUTPUT.PUT_LINE('Doctor: ' || v_diagnosis.doctor_name);
            DBMS_OUTPUT.PUT_LINE('----------------------------------------');
        END LOOP;
        CLOSE c_diagnoses;
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            DBMS_OUTPUT.PUT_LINE('Patient not found');
        WHEN OTHERS THEN
            IF c_diagnoses%ISOPEN THEN
                CLOSE c_diagnoses;
            END IF;
            RAISE;
    END get_recent_diagnoses;
END med_records_mgmt;
/
```

### Transaction Example

```sql
-- Example transaction for patient registration and appointment scheduling
DECLARE
    v_patient_id NUMBER;
    v_appointment_id NUMBER;
    v_doctor_id NUMBER := 1;  -- Assuming doctor with ID 1 exists
BEGIN
    -- Begin transaction
    SAVEPOINT start_transaction;
    
    -- Register new patient
    patient_mgmt.register_patient(
        p_first_name => 'Robert',
        p_last_name => 'Williams',
        p_dob => TO_DATE('12-May-1978', 'DD-MON-YYYY'),
        p_gender => 'Male',
        p_blood_group => 'A+',
        p_address => '456 Oak Avenue, Townsville',
        p_contact => '+1234567896',
        p_email => 'robert.williams@email.com',
        p_emergency => '+1234567897',
        p_insurance => 'Medicare #87654321',
        p_patient_id => v_patient_id
    );
    
    -- Schedule appointment for the new patient
    patient_mgmt.schedule_appointment(
        p_patient_id => v_patient_id,
        p_doctor_id => v_doctor_id,
        p_appt_date => TRUNC(SYSDATE) + 5,  -- 5 days from now
        p_appt_time => SYSTIMESTAMP + 5,    -- 5 days from now
        p_purpose => 'Initial consultation',
        p_appointment_id => v_appointment_id
    );
    
    -- Commit transaction if everything was successful
    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Patient registered and appointment scheduled successfully.');
    DBMS_OUTPUT.PUT_LINE('Patient ID: ' || v_patient_id);
    DBMS_OUTPUT.PUT_LINE('Appointment ID: ' || v_appointment_id);
    
EXCEPTION
    WHEN OTHERS THEN
        -- Rollback transaction if any error occurred
        ROLLBACK TO start_transaction;
        DBMS_OUTPUT.PUT_LINE('Error occurred: ' || SQLERRM);
END;
/
```

### Analytics Query with Window Functions

```sql
-- Analytics: Appointments per doctor with ranking and department information
SELECT
    d.doctor_id,
    d.first_name || ' ' || d.last_name AS doctor_name,
    dep.department_name,
    COUNT(a.appointment_id) AS appointment_count,
    RANK() OVER (ORDER BY COUNT(a.appointment_id) DESC) AS rank_by_appointments,
    RANK() OVER (PARTITION BY dep.department_id ORDER BY COUNT(a.appointment_id) DESC) AS rank_within_department,
    ROUND(COUNT(a.appointment_id) / SUM(COUNT(a.appointment_id)) OVER () * 100, 2) AS percentage_of_total
FROM
    Doctors d
JOIN
    Departments dep ON d.department_id = dep.department_id
LEFT JOIN
    Appointments a ON d.doctor_id = a.doctor_id
GROUP BY
    d.doctor_id, d.first_name, d.last_name, dep.department_name, dep.department_id
ORDER BY
    appointment_count DESC;
```

## Phase 7: Advanced Database Programming and Auditing

### Problem Statement for Advanced Features

The Hospital Management System requires advanced security and auditing mechanisms to ensure data integrity, compliance with healthcare regulations, and proper tracking of system usage. Specifically, the system needs to restrict database modifications during weekdays and public holidays to prevent interference with critical hospital operations. All attempted operations should be logged for audit purposes.

### Trigger Implementation

#### Restriction Trigger for Weekdays and Holidays

```sql
CREATE OR REPLACE TRIGGER restrict_operations_trigger
BEFORE INSERT OR UPDATE OR DELETE ON Patients
BEFORE INSERT OR UPDATE OR DELETE ON Doctors
BEFORE INSERT OR UPDATE OR DELETE ON Appointments
BEFORE INSERT OR UPDATE OR DELETE ON Medical_Records
BEFORE INSERT OR UPDATE OR DELETE ON Medications
BEFORE INSERT OR UPDATE OR DELETE ON Prescriptions
BEGIN
    DECLARE
        v_day_of_week VARCHAR2(20);
        v_is_holiday NUMBER := 0;
        v_current_date DATE := TRUNC(SYSDATE);
        v_user VARCHAR