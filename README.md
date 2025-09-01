# COVID Vaccination System

A comprehensive Laravel 12 web application for managing COVID-19 testing and vaccination appointments. This system provides a platform for patients to book appointments with hospitals and for hospitals to manage patient records and appointments.

## Features

### For Patients
- **Registration & Login**: Secure patient registration and authentication
- **Hospital Search**: Find and browse available hospitals
- **Appointment Booking**: Book COVID-19 tests and vaccinations
- **Appointment Management**: View and track appointment status
- **Test Results**: Access COVID-19 test results
- **Vaccination Records**: View vaccination history
- **Profile Management**: Update personal information

### For Hospitals
- **Registration & Approval**: Hospital registration with admin approval
- **Patient Management**: View and manage patient records
- **Appointment Management**: Handle appointment requests and updates
- **Test Results**: Record and update COVID-19 test results
- **Vaccination Records**: Manage vaccination records
- **Dashboard**: Comprehensive statistics and overview

### For Administrators
- **System Overview**: Complete system statistics and monitoring
- **Patient Management**: View and manage all patient records
- **Hospital Approval**: Approve or reject hospital registrations
- **Report Generation**: Generate detailed reports with export functionality
- **Vaccine Management**: Manage vaccine inventory and availability
- **Booking Details**: Monitor all appointment bookings

## Technology Stack

- **Backend**: Laravel 12 (PHP 8.2+)
- **Frontend**: Blade Templates with Bootstrap 5
- **Database**: MySQL/PostgreSQL
- **Authentication**: Session-based authentication
- **Icons**: Bootstrap Icons
- **Styling**: Custom CSS with Bootstrap components

## Installation

### Prerequisites
- PHP 8.2 or higher
- Composer
- MySQL/PostgreSQL
- Web server (Apache/Nginx)

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd covid-vaccination-system
   ```

2. **Install dependencies**
   ```bash
   composer install
   ```

3. **Environment setup**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

4. **Configure database**
   Edit `.env` file and set your database credentials:
   ```env
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=covid_vaccination_system
   DB_USERNAME=your_username
   DB_PASSWORD=your_password
   ```

5. **Run migrations**
   ```bash
   php artisan migrate
   ```

6. **Start the development server**
   ```bash
   php artisan serve
   ```

7. **Access the application**
   Open your browser and navigate to `http://localhost:8000`

## Database Schema

### Core Tables
- **hospitals**: Hospital information and credentials
- **patients**: Patient information and credentials
- **vaccines**: Available vaccines and stock information
- **appointments**: Appointment bookings and status
- **covid_test_results**: COVID-19 test results
- **vaccination_records**: Vaccination history and records

### Key Relationships
- Patients can have multiple appointments
- Hospitals can have multiple appointments
- Appointments can have one test result or vaccination record
- Vaccination records are linked to specific vaccines

## User Roles & Permissions

### Patient
- Register and login
- Search hospitals
- Book appointments
- View test results and vaccination records
- Manage profile

### Hospital
- Register (requires admin approval)
- Login (after approval)
- Manage patient appointments
- Update test results and vaccination records
- View patient information

### Administrator
- Access to all system data
- Approve/reject hospital registrations
- Generate reports
- Manage vaccines
- Monitor system activity

## API Endpoints

### Public Routes
- `GET /` - Home page
- `GET /user-type` - User type selection
- `GET /patient/register` - Patient registration
- `POST /patient/register` - Patient registration
- `GET /patient/login` - Patient login
- `POST /patient/login` - Patient login
- `GET /hospital/register` - Hospital registration
- `POST /hospital/register` - Hospital registration
- `GET /hospital/login` - Hospital login
- `POST /hospital/login` - Hospital login

### Patient Routes (Authenticated)
- `GET /patient/dashboard` - Patient dashboard
- `GET /patient/hospitals` - Search hospitals
- `GET /patient/hospitals/{id}` - View hospital details
- `GET /patient/hospitals/{id}/book` - Book appointment form
- `POST /patient/hospitals/{id}/book` - Book appointment
- `GET /patient/appointments` - View appointments
- `GET /patient/appointments/{id}` - View appointment details
- `GET /patient/results/covid-tests` - View test results
- `GET /patient/results/vaccinations` - View vaccination records
- `GET /patient/profile` - View profile
- `POST /patient/profile` - Update profile

### Hospital Routes (Authenticated)
- `GET /hospital/dashboard` - Hospital dashboard
- `GET /hospital/patients` - View patients
- `GET /hospital/patients/{id}` - View patient details
- `GET /hospital/appointments` - View appointments
- `GET /hospital/appointments/{id}` - View appointment details
- `PUT /hospital/appointments/{id}/status` - Update appointment status
- `GET /hospital/appointments/{id}/covid-test` - COVID test form
- `POST /hospital/appointments/{id}/covid-test` - Record test result
- `GET /hospital/appointments/{id}/vaccination` - Vaccination form
- `POST /hospital/appointments/{id}/vaccination` - Record vaccination
- `GET /hospital/profile` - View profile
- `POST /hospital/profile` - Update profile

### Admin Routes
- `GET /admin/dashboard` - Admin dashboard
- `GET /admin/patients` - View all patients
- `GET /admin/patients/{id}` - View patient details
- `GET /admin/hospitals` - View all hospitals
- `POST /admin/hospitals/{id}/status` - Update hospital status
- `GET /admin/reports` - View reports
- `GET /admin/reports/export` - Export reports
- `GET /admin/vaccines` - Manage vaccines
- `POST /admin/vaccines` - Add vaccine
- `PUT /admin/vaccines/{id}` - Update vaccine
- `GET /admin/bookings` - View all bookings
- `GET /admin/bookings/{id}` - View booking details

## Security Features

- **Password Hashing**: All passwords are hashed using Laravel's Hash facade
- **CSRF Protection**: All forms include CSRF tokens
- **Session Management**: Secure session-based authentication
- **Input Validation**: Comprehensive server-side validation
- **SQL Injection Protection**: Laravel's Eloquent ORM provides protection
- **XSS Protection**: Blade templating engine escapes output

## File Structure

```
covid-vaccination-system/
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── AdminController.php
│   │   │   ├── HospitalController.php
│   │   │   ├── PatientController.php
│   │   │   └── HomeController.php
│   │   └── Middleware/
│   │       ├── HospitalAuth.php
│   │       └── PatientAuth.php
│   ├── Models/
│   │   ├── Hospital.php
│   │   ├── Patient.php
│   │   ├── Vaccine.php
│   │   ├── Appointment.php
│   │   ├── CovidTestResult.php
│   │   └── VaccinationRecord.php
│   └── Providers/
├── database/
│   └── migrations/
│       ├── create_hospitals_table.php
│       ├── create_patients_table.php
│       ├── create_vaccines_table.php
│       ├── create_appointments_table.php
│       ├── create_covid_test_results_table.php
│       └── create_vaccination_records_table.php
├── resources/
│   └── views/
│       ├── layouts/
│       │   └── app.blade.php
│       ├── home.blade.php
│       ├── user-type.blade.php
│       ├── patient/
│       │   ├── register.blade.php
│       │   ├── login.blade.php
│       │   ├── dashboard.blade.php
│       │   ├── hospitals/
│       │   │   ├── search.blade.php
│       │   │   └── show.blade.php
│       │   └── appointments/
│       │       ├── index.blade.php
│       │       └── create.blade.php
│       ├── hospital/
│       │   ├── register.blade.php
│       │   ├── login.blade.php
│       │   └── dashboard.blade.php
│       └── admin/
│           └── dashboard.blade.php
├── routes/
│   └── web.php
├── public/
├── storage/
├── tests/
├── .env
├── .env.example
├── composer.json
└── README.md
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support and questions, please contact the development team or create an issue in the repository.

## Acknowledgments

- Laravel team for the excellent framework
- Bootstrap team for the UI components
- All contributors and testers

---

**Note**: This is a demonstration system. For production use, additional security measures, error handling, and testing should be implemented.
