#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>
#include <conio.h>

using namespace std;

struct Record {
    string cnicDigits;
    string name;
    int numOfDoses;
    string typeOfDose;
    string currentStatus;
    string gender;
};

const int MAX_RECORDS = 100;
Record records[MAX_RECORDS];
int numRecords = 0;
const string FILENAME = "vaccination_records.txt";

void saveRecordsToFile() {
    ofstream file(FILENAME);
    if (!file) {
        cout << "Error opening file for writing." << endl;
        return;
    }

    for (int i = 0; i < numRecords; i++) {
        file << records[i].cnicDigits << " "
            << records[i].name << " "
            << records[i].gender << " "
            << records[i].numOfDoses << " "
            << records[i].typeOfDose << " "
            << records[i].currentStatus << "\n";
    }

    file.close();
}

void loadRecordsFromFile() {
    ifstream file(FILENAME);
    if (!file) {
        cout << "No existing records found. Starting fresh." << endl;
        return;
    }

    numRecords = 0;
    while (file >> records[numRecords].cnicDigits
        >> records[numRecords].name
        >> records[numRecords].gender
        >> records[numRecords].numOfDoses
        >> records[numRecords].typeOfDose
        >> records[numRecords].currentStatus) {
        numRecords++;
        if (numRecords >= MAX_RECORDS) {
            cout << "Maximum number of records reached from file." << endl;
            break;
        }
    }

    file.close();
}

void createRecord() {
    if (numRecords >= MAX_RECORDS) {
        cout << "Maximum number of records reached." << endl;
        return;
    }

    Record newRecord;
    cout << "Enter CNIC digits (last 4): ";
    cin >> newRecord.cnicDigits;
    cout << "Enter name: ";
    cin >> newRecord.name;
    cout << "Enter gender (M/F): ";
    cin >> newRecord.gender;
    cout << "Enter type of dose: ";
    cin >> newRecord.typeOfDose;
    cout << "Enter number of doses: ";
    cin >> newRecord.numOfDoses;
    cout << "Enter current status: ";
    cin >> newRecord.currentStatus;

    records[numRecords] = newRecord;
    numRecords++;

    saveRecordsToFile(); // Save the updated records to file

    cout << "\nRecord created successfully!\n" << endl;
}

void searchRecord() {
    if (numRecords == 0) {
        cout << "No records available." << endl;
        return;
    }

    string cnic;
    cout << "Enter CNIC digits of the record to search: ";
    cin >> cnic;

    bool found = false;
    for (int i = 0; i < numRecords; i++) {
        if (records[i].cnicDigits == cnic) {
            cout << "\nRecord found:\n" << endl;
            cout << "-----------------------------------" << endl;
            cout << "CNIC digits: " << records[i].cnicDigits << endl;
            cout << "Name: " << records[i].name << endl;
            cout << "Gender: " << records[i].gender << endl;
            cout << "Number of doses: " << records[i].numOfDoses << endl;
            cout << "Type of dose: " << records[i].typeOfDose << endl;
            cout << "Current status: " << records[i].currentStatus << endl;
            cout << "-----------------------------------" << endl;
            found = true;
            break;
        }
    }

    if (!found) {
        cout << "\nRecord not found." << endl;
    }
}

void modifyRecord() {
    if (numRecords == 0) {
        cout << "No records available." << endl;
        return;
    }

    string cnic;
    cout << "Enter CNIC digits of the record to modify: ";
    cin >> cnic;

    bool found = false;
    for (int i = 0; i < numRecords; i++) {
        if (records[i].cnicDigits == cnic) {
            cout << "Enter new CNIC digits (last 4): ";
            cin >> records[i].cnicDigits;
            cout << "Enter new name: ";
            cin >> records[i].name;
            cout << "Enter new gender (M/F): ";
            cin >> records[i].gender;
            cout << "Enter new number of doses: ";
            cin >> records[i].numOfDoses;
            cout << "Enter new type of dose: ";
            cin >> records[i].typeOfDose;
            cout << "Enter new current status: ";
            cin >> records[i].currentStatus;
            found = true;
            break;
        }
    }

    if (found) {
        saveRecordsToFile(); // Save the updated records to file
        cout << "\nRecord modified successfully!" << endl;
    }
    else {
        cout << "\nRecord not found." << endl;
    }
}

void deleteRecord() {
    if (numRecords == 0) {
        cout << "No records available." << endl;
        return;
    }

    string cnic;
    cout << "Enter CNIC digits of the record to delete: ";
    cin >> cnic;

    bool found = false;
    for (int i = 0; i < numRecords; i++) {
        if (records[i].cnicDigits == cnic) {
            for (int j = i; j < numRecords - 1; j++) {
                records[j] = records[j + 1];
            }
            numRecords--;
            found = true;
            break;
        }
    }

    if (found) {
        saveRecordsToFile(); // Save the updated records to file
        cout << "\nRecord deleted successfully!" << endl;
    }
    else {
        cout << "\nRecord not found." << endl;
    }
}

void displayFrontPage() {
    for (int i = 0; i < 10; i++) {
        cout << endl;
    }
    for (int i = 0; i < 120; i++) {
        cout << "*";
    }
    cout << endl;
    cout << setw(78) << "WELCOME TO VACCINATION DATABASE SYSTEM." << endl;
    cout << setw(75) << "PROJECT BY: IBRAHIM AND MEHMOOD" << endl;
    cout << setw(70) << "PRESS ANY KEY TO CONTINUE." << endl;
    cout << endl;
    for (int i = 0; i < 120; i++) {
        cout << "*";
    }
    _getch();
    system("CLS");
}

void displayMenu() {
    cout << "------------------------------------------------------------" << endl;
    cout << "|                    VACCINATION DATABASE                  |" << endl;
    cout << "------------------------------------------------------------" << endl;
    cout << "| 1. Create Record                                          |" << endl;
    cout << "| 2. Search Record                                          |" << endl;
    cout << "| 3. Modify Record                                          |" << endl;
    cout << "| 4. Delete Record                                          |" << endl;
    cout << "| 5. Exit                                                   |" << endl;
    cout << "------------------------------------------------------------" << endl;
    cout << "Enter your choice: ";
}

int main() {
    system("CLS");
    displayFrontPage();

    loadRecordsFromFile(); // Load records from the file at the start

    int choice;
    do {
        displayMenu();
        cin >> choice;

        system("CLS");

        switch (choice) {
        case 1:
            createRecord();
            break;
        case 2:
            searchRecord();
            break;
        case 3:
            modifyRecord();
            break;
        case 4:
            deleteRecord();
            break;
        case 5:
            cout << "Exited program successfully." << endl;
            break;
        default:
            cout << "Invalid choice. Please try again." << endl;
            break;
        }
    } while (choice != 5);

    return 0;
}
