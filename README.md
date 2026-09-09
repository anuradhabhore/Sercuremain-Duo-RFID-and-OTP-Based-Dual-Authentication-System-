# 🔐 SecurePass Duo – RFID & OTP Based Dual Authentication System

## 📌 Project Overview

**SecurePass Duo** is an embedded security system that uses **RFID and OTP-based dual authentication**. The system is developed using the **LPC2148 ARM7 microcontroller, RFID reader, GSM module, keypad, and LCD**.

The system provides authentication only when **both RFID verification and OTP verification are successful**.


## 🎯 Objectives

* Provide two-level authentication for improved security.
* Authenticate users using an authorized RFID card.
* Generate a temporary OTP after successful RFID verification.
* Send the OTP to the registered mobile number through GSM.
* Allow the user to enter the OTP using a keypad.
* Verify the entered OTP.
* Display authentication status on the LCD.
* Prevent unauthorized authentication.



## ⚙️ Working Principle

The system follows two authentication levels:


RFID Card
    ↓
RFID Reader
    ↓
UART Communication
    ↓
LPC2148
    ↓
Verify RFID UID
    ↓
RFID Valid?
   /       \
 No         Yes
 ↓           ↓
Authentication   Generate
Denied           OTP
                  ↓
             GSM Module
                  ↓
             SMS to User
                  ↓
              Enter OTP
                  ↓
                Keypad
                  ↓
               LPC2148
                  ↓
              Verify OTP
               /      \
             No        Yes
             ↓          ↓
       Authentication  Authentication
            Denied       Successful
                         ↓
                    LCD Display

##  PROJECT BLOCK DIAGRAM


<img width="1093" height="728" alt="image" src="https://github.com/user-attachments/assets/3e83cefc-c00c-4fcf-b29e-5af976ce2ac2" />


## 🔑 Authentication Process

### 1. RFID Authentication

The user places an RFID card near the RFID reader.

The RFID reader reads the card's UID and communicates with **LPC2148 through SPI**.

LPC2148 compares the received RFID UID with the authorized UID stored in memory.


RFID UID → LPC2148 → Compare with Authorized UID


If the UID matches, the system proceeds to OTP authentication.



### 2. OTP Generation

After successful RFID verification, LPC2148 generates a temporary OTP.

Example:


OTP = 583214


The OTP is temporarily stored for verification.



### 3. OTP Transmission

LPC2148 communicates with the GSM module through **UART**.

AT commands are used to control the GSM module and send the OTP through SMS.


LPC2148
    ↓ UART
GSM Module
    ↓
Cellular Network
    ↓
Registered Mobile




### 4. OTP Verification

The user receives the OTP on the registered mobile and enters it using the keypad.

LPC2148 compares the generated OTP with the entered OTP.


Generated OTP
      ↓
Entered OTP
      ↓
   Compare


If both values match, authentication is successful.



### 5. Authentication Result

If both RFID and OTP are valid:


RFID = VALID
OTP  = VALID
     ↓
AUTHENTICATION SUCCESSFUL


The LCD displays the successful authentication message.

If either authentication fails:


RFID = INVALID
       OR
OTP = INVALID
       ↓
AUTHENTICATION FAILED




## 🧩 Hardware Components

| Component         | Function               |
| ----------------- | ---------------------- |
| **LPC2148**       | Main controller        |
| **RFID Reader**   | Reads RFID card/tag    |
| **RFID Card/Tag** | User identification    |
| **GSM Module**    | Sends OTP through SMS  |
| **SIM Card**      | Cellular communication |
| **4×4 Keypad**    | OTP input              |
| **16×2 LCD**      | Displays system status |
| **Power Supply**  | Powers the system      |



## 🔌 Communication Protocols

| Device                | Protocol/Interface          |
| --------------------- | --------------------------- |
| RFID Reader ↔ LPC2148 | **UART**                     |
| GSM Module ↔ LPC2148  | **UART**                    |
| Keypad ↔ LPC2148      | **GPIO**                    |
| LCD ↔ LPC2148         | **GPIO/Parallel Interface** |

> **Note:** SPI applies when using an SPI-based RFID reader such as the MFRC522. The exact interface depends on the RFID reader model.


## 💻 Software & Technologies

* **Embedded C**
* **LPC2148 ARM7**
* **Keil µVision**
* **UART**
* **GPIO**
* **RFID**
* **GSM**
* **AT Commands**



## 🛡️ Security Features

* Two-factor authentication
* Authorized RFID card verification
* Temporary OTP authentication
* SMS-based OTP delivery
* OTP verification
* Authentication failure for invalid RFID
* Authentication failure for incorrect OTP
* OTP expiry can be implemented
* Limited OTP attempts can be implemented
* Authentication status displayed on LCD



## 📊 System Architecture


                  ┌───────────────┐
                  │   RFID Card   │
                  └───────┬───────┘
                          ↓
                  ┌───────────────┐
                  │ RFID Reader   │
                  └───────┬───────┘
                          │
                         UART
                          │
                          ↓
                  ┌────────────────┐
                  │    LPC2148     │
                  │    ARM7 MCU    │
                  └───┬────────┬───┘
                      │        │
                 UART │        │ GPIO
                      ↓        ↓
               ┌─────────┐  ┌─────────┐
               │   GSM   │  │ Keypad  │
               │ Module  │  └─────────┘
               └────┬────┘
                    │
                   SMS
                    ↓
              ┌───────────┐
              │  Mobile   │
              │   Phone   │
              └───────────┘

                  LPC2148
                     │
                     │ GPIO
                     ↓
                ┌─────────┐
                │   LCD   │
                └─────────┘




## 📱 Example LCD Messages

### System Start


SECUREPASS DUO
SCAN RFID

### RFID Verification


RFID DETECTED
VERIFYING...


### Successful RFID


RFID VERIFIED
OTP SENT

### OTP Entry


ENTER OTP:
******


### Successful Authentication


AUTH SUCCESS
WELCOME

### Authentication Failure


AUTH FAILED
TRY AGAIN




## 🔄 Authentication Logic


IF RFID is valid
        ↓
Generate OTP
        ↓
Send OTP through GSM
        ↓
User enters OTP
        ↓
IF OTP is correct
        ↓
Authentication Successful
        ↓
Display Success on LCD
ELSE
        ↓
Authentication Failed
        ↓
Display Failure on LCD
```

---

## 🚨 Error Handling

### Invalid RFID


Invalid RFID
     ↓
Authentication Failed
```

### Incorrect OTP


Wrong OTP
     ↓
Authentication Failed
```

### OTP Timeout


OTP Expired
     ↓
Generate New OTP
```

### GSM Failure


GSM/Network Error
     ↓
Retry / Display Error
```

---

## 🌐 Applications

SecurePass Duo can be used for:

* Office authentication systems
* Restricted-area authentication
* College and industrial laboratories
* Industrial security systems
* Secure storage areas
* Server-room authentication
* Locker authentication
* Educational institutions

---

## ✅ Advantages

* Provides two-factor authentication.
* Contactless RFID identification.
* OTP provides a second authentication factor.
* GSM allows OTP delivery through SMS.
* Simple embedded hardware architecture.
* LCD provides authentication status.
* Can be expanded with additional security features.

---

## 🔮 Future Enhancements

* Add biometric authentication.
* Add IoT/cloud-based monitoring.
* Maintain authentication logs.
* Add RTC for date and time recording.
* Add administrator authentication.
* Add tamper detection.
* Implement secure cryptographic OTP generation.
* Add a mobile application.
* Add multiple-user support.
* Add stronger RFID authentication.

---

## 📁 Suggested Project Structure


SecurePass-Duo/
│
├── README.md
│
├── src/
│   ├── main.c
│   ├── rfid.c
│   ├── rfid.h
│   ├── gsm.c
│   ├── gsm.h
│   ├── uart.c
│   ├── uart.h
│   ├── spi.c
│   ├── spi.h
│   ├── keypad.c
│   ├── keypad.h
│   ├── lcd.c
│   ├── lcd.h
│   ├── otp.c
│   └── otp.h
│
├── circuit/
│   └── circuit_diagram.png
│
├── images/
│   ├── project_setup.jpg
│   └── block_diagram.png
│
└── docs/
    └── project_report.pdf




## 👩‍💻 Skills Demonstrated

This project demonstrates practical knowledge of:

* Embedded C programming
* ARM7/LPC2148 programming
* Microcontroller interfacing
* UART communication
* GSM AT commands
* RFID interfacing
* Keypad interfacing
* LCD interfacing
* GPIO programming
* Authentication logic
* Hardware-software integration
* Embedded security concepts

---

## 📌 Project Summary

**SecurePass Duo** is an RFID and OTP-based dual authentication system using **LPC2148**. The RFID reader communicates with LPC2148 through , while the GSM module communicates through **UART**. After successful RFID verification, an OTP is sent to the registered mobile number. The user enters the OTP through the keypad, and authentication is successful only when both RFID and OTP are valid.

---

## ⭐ Key Concept


        RFID
          +
        OTP
          ↓
   DUAL AUTHENTICATION
          ↓
 AUTHENTICATION RESULT
          ↓
       LCD DISPLAY


**RFID Valid + OTP Valid → Authentication Successful**

**RFID Invalid OR OTP Invalid → Authentication Failed**
