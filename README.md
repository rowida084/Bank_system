#include <iostream>
#include <string>
#include<vector>
#include<fstream>
#include<iomanip>
using namespace std;
struct acountInfo {
	string acountNumber;
	string  pinCode;
	string  clientName;
	string  phone;
	float  balance = 0.0;
	bool deleted = false;
};
enum enMenuOptions {
	showClientList = 1, addNewClient = 2, deleteClient = 3, updataClientInfo = 4,
	findClient = 5, exist = 6
};

string convertToString(acountInfo acount, string delim) {
	string s = "";
	s += acount.acountNumber + delim;
	s += acount.pinCode + delim;
	s += acount.clientName + delim;
	s += acount.phone + delim;
	s += to_string(acount.balance);
	return s;
}

void saveVectorToFile(vector<acountInfo>& acounts) {
	fstream dataFile;
	string line;
	dataFile.open("dataFile.txt", ios::out);
	if (dataFile.is_open()) {
		for (acountInfo& client : acounts) {
			if (client.deleted == false) {
				line = convertToString(client, "#//#");
				dataFile << line << endl;
			}
		}
	}
	dataFile.close();
}


vector<string> split(string delim, string s) {
	vector <string > acount;
	string word = "";
	int pos = 0;
	while ((pos = s.find(delim)) != std::string::npos) {
		word = s.substr(0, pos);
		if (word != "") {
			acount.push_back(word);

		}s = s.erase(0, pos + delim.length());
	}

	if (s != "") {
		acount.push_back(s);
	}
	return acount;
}

acountInfo converLineToVector(string line, string delim = "#//#") {
	acountInfo acount;
	vector<string> vClientAcount;
	vClientAcount = split(delim, line);
	acount.acountNumber = vClientAcount[0];
	acount.pinCode = vClientAcount[1];
	acount.clientName = vClientAcount[2];
	acount.phone = vClientAcount[3];
	acount.balance = stod(vClientAcount[4]);
	return acount;
}

vector<acountInfo> loadAcountsInfoFromFile() {
	vector<acountInfo> Vacounts;
	fstream dataFile;
	dataFile.open("dataFile.txt", ios::in);
	if (dataFile.is_open()) {
		string line;
		acountInfo acount;
		while (getline(dataFile, line)) {
			acount = converLineToVector(line, "#//#");
			Vacounts.push_back(acount);
		}
	}
	dataFile.close();
	return Vacounts;
}

void menu() {
	system("cls");
	cout << "------------------------------------------------" << endl;
	cout << "                Main menu screen                  " << endl;
	cout << "------------------------------------------------" << endl;
	cout << "[1] Show Client List" << endl;
	cout << "[2] Add New Client" << endl;
	cout << "[3] Delet Client" << endl;
	cout << "[4] Update Client Info" << endl;
	cout << "[5] Find Client" << endl;
	cout << "[6] Transactions" << endl;
	cout << "[7] Exist" << endl;
	cout << "---------------------------------------------------" << endl;
}

void printClientData(acountInfo acount) {
	cout << "|  " << left << setw(20) << acount.acountNumber;
	cout << "|  " << left << setw(20) << acount.pinCode;
	cout << "|  " << left << setw(20) << acount.clientName;
	cout << "|  " << left << setw(20) << acount.phone;
	cout << "|  " << left << setw(20) << acount.balance;
	cout << endl;
}

void showClientsInfo(vector<acountInfo>acounts) {
	cout << "|  " << left << setw(20) << "Acount number";
	cout << "|  " << left << setw(20) << "Pin code";
	cout << "|  " << left << setw(20) << "Name";
	cout << "|  " << left << setw(20) << "phone";
	cout << "|  " << left << setw(20) << "Balance" << endl;
	cout << endl;
	for (acountInfo acount : acounts) {
		printClientData(acount);
		cout << endl;
	}
}


void loadFile(vector<acountInfo> acounts, string fileName) {
	fstream dataFile;
	dataFile.open("dataFile.txt", ios::in);
	if (dataFile.is_open()) {

	}
}

acountInfo  readClient() {
	acountInfo acount;
	cout << "Enter Acount Number? ";
	getline(cin >> ws, acount.acountNumber);
	cout << "Enter Pin Code? ";
	getline(cin, acount.pinCode);
	cout << "Enter Name? ";
	getline(cin, acount.clientName);
	cout << "Enter Phone? ";
	getline(cin, acount.phone);
	cout << "Enter Acount Balance? ";
	cin >> acount.balance;
	return acount;
}

acountInfo readAcount(vector<acountInfo> acounts) {
	acountInfo acount;
	cout << "Enter acount Number?";
	getline(cin >> ws, acount.acountNumber);

	for (acountInfo& c : acounts) {
		while (c.acountNumber == acount.acountNumber) {

			cout << "this acount already exists ! Enter acount Number again?" << endl;

			getline(cin >> ws, acount.acountNumber);
		}
	}


	cout << "Enter Pin Code? ";
	getline(cin, acount.pinCode);
	cout << "Enter Name? ";
	getline(cin, acount.clientName);
	cout << "Enter Phone? ";
	getline(cin, acount.phone);
	cout << "Enter Acount Balance? ";
	cin >> acount.balance;
	return acount;
}

void addClient(vector<acountInfo>& acounts) {
	acountInfo acount;
	acount = readAcount(acounts);
	acounts.push_back(acount);
	saveVectorToFile(acounts);

}

string readAcountNumber() {
	string acountNumber;
	cout << "Please enter the count number ?";
	cin >> acountNumber;
	return acountNumber;
}

void addMoreClients(vector<acountInfo>& acounts) {
	char answer = 'y';
	do {
		system("cls");
		addClient(acounts);
		cout << "The client added successfuly! Do you want add more?";
		cin >> answer;
	} while (answer == 'y' || answer == 'Y');
}

bool  findClientNumber(string acountNumber, vector<acountInfo>& Vacounts, acountInfo& acount) {
	for (acountInfo& c : Vacounts) {
		if (acountNumber == c.acountNumber) {
			acount = c;
			return true;
		}
	}
	return false;
}


void showAcountDetailsByAcountNumber(vector<acountInfo>& acounts, string acountNumber) {
	acountInfo acount;
	acounts = loadAcountsInfoFromFile();
	//showClientsInfo(acounts);

	if (findClientNumber(acountNumber, acounts, acount)) {
		cout << "The following are client details: " << endl;
		printClientData(acount);
	}
	else {
		cout << "Client with acount number " << acountNumber << " is not found" << endl;
	}
}

bool  markClientDeleted(vector<acountInfo>& acounts, string acountNumber) {
	for (acountInfo& client : acounts) {
		if (findClientNumber(acountNumber, acounts, client)) {
			client.deleted = true;
			return true;
		}
	}
	return false;
}

void deleteAcount(string acountNumber, vector<acountInfo>& acounts) {
	char answer = 'n';
	acountInfo client;
	if (findClientNumber(acountNumber, acounts, client)) {
		printClientData(client);
		cout << "Are you sure you want delete this client" << endl;
		cin >> answer;
		if (answer == 'y' || answer == 'Y') {
			markClientDeleted(acounts, acountNumber);
			saveVectorToFile(acounts);
			cout << "/n/nThe acount is deleted successfuly./n/n";
		}
	}
	else {
		cout << "/n/nThis acount with " << acountNumber << " does not exist/n/n";
	}
}

acountInfo readUpdateClientInfo(string acountNumber) {
	acountInfo client;
	client.acountNumber = acountNumber;
	cout << "Enter pincode?";
	getline(cin >> ws, client.pinCode);
	cout << "Enter name?";
	getline(cin, client.clientName);
	cout << "Enter phone?";
	getline(cin, client.phone);
	cout << "Enter Balance?";
	cin >> client.balance;
	return client;
}

void updateClient(string acountNumber, vector<acountInfo>acounts) {
	char answer = 'n';
	acountInfo client;
	if (findClientNumber(acountNumber, acounts, client)) {
		printClientData(client);
		cout << "Are you sure you want update this client?";
		cin >> answer;
		if (answer == 'y' || answer == 'Y') {
			for (acountInfo& c : acounts) {
				if (c.acountNumber == acountNumber) {
					c = readUpdateClientInfo(acountNumber);
				}break;
			}

			saveVectorToFile(acounts);
			cout << "client is updated succissfully" << endl;
		}
	}
	else {
		cout << "client with number " << acountNumber << " is not exist" << endl;
	}
}

void transactionMenu() {
	cout << "==============================================================" << endl;
	cout << "                      Transaction Menu                           " << endl;
	cout << "==============================================================" << endl;
	cout << "[1] Deposite." << endl;
	cout << "[2] Withdraw." << endl;
	cout << "[3] Total Balance." << endl;
	cout << "[4] Main Menu." << endl;
}

bool depositeBalance(double depositeAmount, vector<acountInfo>acounts, string acountNumber) {
	char answer = 'n';
	cout << "Are you sure you want perform this transactin? [y/n] ?";
	cin >> answer;
	if (answer == 'y' || answer == 'Y') {
		for (acountInfo& c : acounts) {
			if (acountNumber == c.acountNumber) {
				c.balance += depositeAmount;
				saveVectorToFile(acounts);
				return true;
			}
		}
	}
	return false;
}

void deposite(vector<acountInfo> & acounts) {
	/*vector<acountInfo> acounts;
	acounts = loadAcountsInfoFromFile();*/
	string acountNumber;
	double depositeAmount=0;
	acountInfo acount;
	cout << "Please Enter AcountNumber?";
	acountNumber = readAcountNumber();
	while (!findClientNumber(acountNumber, acounts, acount)) {
		cout << "This acount does not exist" << endl;
		acountNumber = readAcountNumber();
	}
	printClientData(acount);
	cout << "Please Enter deposite amount? ";
	cin >> depositeAmount;
	if (depositeBalance(depositeAmount, acounts, acountNumber)) {
		cout << "Done Successfuly" << endl;
	}
	

}

void printTotalBalances(acountInfo acount) {
	cout << "|  " << left << setw(20) << acount.acountNumber;
	cout << "|  " << left << setw(20) << acount.clientName;
	cout << "|  " << left << setw(20) << acount.balance;
	cout << endl;
}

void showTotalBalances(vector<acountInfo>acounts) {
	cout << "|  " << left << setw(20) << "Acount number";
	cout << "|  " << left << setw(20) << "Name";
	cout << "|  " << left << setw(20) << "Balance" << endl;
	cout << endl;
	double totalBalances = 0;
	for (acountInfo acount : acounts) {
		printTotalBalances(acount);
		totalBalances += acount.balance;
		cout << endl;
	}
	cout << "Total Balances = " << totalBalances << endl;
}

void goBackToTransactionMenu() {
	cout << "Press eny key to go back menu" << endl;
	system("pause > 0");
	transactionMenu();
}

void goBackMainMenu() {
	cout << "Press eny key to go back menu" << endl;
	system("pause>0");
	menu();
}

void showEndScreen() {
	cout << "------------------------------------" << endl;
	cout << "Program Ends):" << endl;
	cout << "------------------------------------" << endl;

}

bool withdraw(vector<acountInfo> &acounts,string acountNumber,double amount ) {
	char answer = 'n';
	cout << "Are you sure you want perform this transactin? [y/n] ?";
	cin >> answer;
	if (answer == 'y' || answer == 'Y') {
		for (acountInfo& c : acounts) {
			if (acountNumber == c.acountNumber&&amount<= c.balance) {
				c.balance += amount ;
				saveVectorToFile(acounts);
				return true;
			}
		}
	}
	return false;
}

void withdrawPerformance(vector<acountInfo>&acounts) {
		/*vector<acountInfo> acounts;
		acounts = loadAcountsInfoFromFile();*/
		string acountNumber;
		double amount = 0;
		acountInfo acount;
		acountNumber = readAcountNumber();
		while (!findClientNumber(acountNumber, acounts, acount)) {
			cout << "This acount does not exist" << endl;
			acountNumber = readAcountNumber();
		}
		printClientData(acount);
		cout << "Please Enter withdraw amount? ";
		cin >> amount;
		if (withdraw(acounts, acountNumber, amount)) {
			cout << "Done Successfuly" << endl;

	}
}

void transactonMenuPerformance(vector<acountInfo>&acounts) {
	char choose;
	transactionMenu();
	char answer = 'y';
	do {
		cout << "Choose what do you want to do?[1 to 4]?";
		cin >> choose;
		switch (choose) {
		case '1': {
			deposite(acounts);
			goBackToTransactionMenu();
			break;
		}
		case'2': {
			withdrawPerformance(acounts);
			goBackToTransactionMenu();
			break;
		}
		case'3': {
			showTotalBalances(acounts);
			goBackToTransactionMenu();
			break;
		}
		case'4': {
			showEndScreen();
			break;
		}
		}
		cout << "Do you want do another operation? ";
		cin >> answer;
	} while (answer == 'y' || answer == 'Y');
}

int main() {
	vector<acountInfo> acounts;
	acounts = loadAcountsInfoFromFile();
	char choose;


	char answer = 'y';
	do {
		menu();

		cout << "Choose what do you want to do? [1 to 7 ]";
		cin >> choose;
		switch (choose) {
		case '1':
			showClientsInfo(acounts);
			//goBackMainMenu();

			break;

		case '2':
			cout << "------------------------------------------" << endl;
			cout << "         Adding new client screen:               " << endl;
			cout << "------------------------------------------" << endl;
			addMoreClients(acounts);
			//goBackMainMenu();

			break;

		case'3':
			cout << "------------------------------------------" << endl;
			cout << "              Delete client screen:         " << endl;
			cout << "------------------------------------------" << endl;

			deleteAcount(readAcountNumber(), acounts);
			//goBackMainMenu();

			break;

		case'4':
			cout << "------------------------------------------" << endl;
			cout << "              Update client screen             " << endl;
			cout << "------------------------------------------" << endl;
			updateClient(readAcountNumber(), acounts);
			//goBackMainMenu();
			break;

		case'5':
			cout << "------------------------------------------" << endl;
			cout << "           Find client screen                 " << endl;
			cout << "------------------------------------------" << endl;
			showAcountDetailsByAcountNumber(acounts, readAcountNumber());
			//goBackMainMenu();
			break;
		case'6': {
			transactonMenuPerformance(acounts);
			goBackMainMenu();
			break;
		}
		case'7': {
			return 0;
			break;
		}
		}
		cout << "Do you want do another operation? ";
		cin >> answer;
	} while (answer == 'y' || answer == 'Y');
}
