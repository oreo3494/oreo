# oreo

<h1>Hi </h1>
<div>oreo says hi!</div>
#include <iostream>
#include <string>
using namespace std;

struct Date {
    int day;
    int month;
    int year;
};

// Helper function to check if a year is a leap year
bool isLeapYear(int year) {
    return (year % 4 == 0 && (year % 100 != 0 || year % 400 == 0));
}

// Helper function to calculate the number of days in a month
int daysInMonth(int month, int year) {
    switch(month) {
        case 1: case 3: case 5: case 7: case 8: case 10: case 12:
            return 31;
        case 4: case 6: case 9: case 11:
            return 30;
        case 2:
            return isLeapYear(year) ? 29 : 28;
        default:
            return 0;
    }
}

// Function to convert a given date to the number of days since 01/01/1900
int dateToDays(const Date &date) {
    int days = 0;
    // Add days for the years since 1900
    for (int year = 1900; year < date.year; year++) {
        days += isLeapYear(year) ? 366 : 365;
    }

    // Add days for the months in the current year
    for (int month = 1; month < date.month; month++) {
        days += daysInMonth(month, date.year);
    }

    // Add days in the current month
    days += date.day;

    return days;
}

class Patient {
private:
    string name;
    string disease;
    Date admissionDate;
    Date dischargeDate;

public:
    void getData() {
        cout << "\nENTER NAME OF PATIENT = ";
        getline(cin, name);

        cout << "ENTER ADMISSION DATE (DD/MM/YYYY): ";
        cin >> admissionDate.day >> admissionDate.month >> admissionDate.year;

        cin.ignore();

        cout << "ENTER DISEASE OF PATIENT = ";
        getline(cin, disease);

        cout << "ENTER DISCHARGE DATE (DD/MM/YYYY): ";
        cin >> dischargeDate.day >> dischargeDate.month >> dischargeDate.year;

        cin.ignore();
    }

    void display() const {
        cout << "\n\nPATIENT NAME = " << name;
        cout << "\nADMISSION DATE = " << admissionDate.day << "/" << admissionDate.month << "/" << admissionDate.year;
        cout << "\nDISEASE = " << disease;
        cout << "\nDISCHARGE DATE = " << dischargeDate.day << "/" << dischargeDate.month << "/" << dischargeDate.year;
    }

    Date getAdmissionDate() const {
        return admissionDate;
    }

    Date getDischargeDate() const {
        return dischargeDate;
    }

    string getName() const {
        return name;
    }

    string getDisease() const {
        return disease;
    }
};

class PatientWithStayDuration : public Patient {
private:
    int stayInDays;

public:
    void calculateStayDuration() {
        int admitDays = dateToDays(getAdmissionDate());
        int dischargeDays = dateToDays(getDischargeDate());
        stayInDays = dischargeDays - admitDays;  // Get the difference in days
    }

    int getStayDuration() const {
        return stayInDays;
    }

    void displayStayDuration() const {
        display();
        cout << "\nDURATION OF STAY = " << stayInDays << " days";
    }
};

int main() {
    int n;
    cout << "\nENTER NUMBER OF PATIENTS = ";
    cin >> n;

    cin.ignore();

    PatientWithStayDuration patients[n];
    for (int i = 0; i < n; i++) {
        cout << "\n\nPATIENT No. " << i + 1;
        patients[i].getData();
        patients[i].calculateStayDuration();
    }

    cout << "\n\nAll Patients:\n";
    for (int i = 0; i < n; i++) {
        cout << "\nPatient " << i + 1 << ":\n";
        patients[i].displayStayDuration();
    }

    return 0;
}