#include <iostream>
#include <iomanip>
#include <vector>
#include <typeinfo>

using namespace std;

class Account{
public:
    //Constructor with argument list of int, string and double
    Account(int AN,string AO,double AB){
        accountNumber = AN;
        accountOwner = AO;

        if(AB<0){
            accountBalance=0;
            cout << "Initial balance was invalid";
        }else{
            accountBalance = AB;
        }

    }

    void credit(double amnt){
        accountBalance = accountBalance + amnt;
    }

    void debit(double withdraw){
        if(withdraw>accountBalance){
            cout << "Debit amount exceeded account balance.";
        }else{
            accountBalance = accountBalance - withdraw;
        }
    }

    const double getBalance(){
        cout << accountBalance;
        return accountBalance;

    }

private:
    int accountNumber;
    string accountOwner;

protected:
    double accountBalance;
};

class SavingsAccount : public Account{
public:
    SavingsAccount(int AN,string AO,double AB,double IR) : Account(AN, AO, AB) {
        interestRate = IR;
    }

    double calculateInterest(){
        return interestRate * accountBalance;

    }

private:
    double interestRate;
};

class CheckingAccount : public Account{
public:
    CheckingAccount(int AN,string AO,double AB,double f) : Account(AN, AO, AB){
        fee = f;
    }
    void credit(double amnt){
        accountBalance = accountBalance + amnt - fee;
    }

    //Case not considered in assignemnt: Sum of withdraw amount and fee exceeds the accountBalance
    void debit(double withdraw){
        if(withdraw>accountBalance){
            cout << "Debit amount exceeded account balance.";
        }else{
            accountBalance = accountBalance - withdraw - fee;
        }
    }
private:
    double fee;
};

int main()
{
    vector<Account*> Account{
        //arguments are act#, act owner, act bal
        new SavingsAccount()
        new CheckingAccount()
        };
    for (Account* AccountPtr* : Account){
        cout << AccountPtr->toString() << endl;

        Account* derivedPtr = dynamic_cast<Account*>(AccountPtr);
        if (derivedPtr != nullptr){
            double OriginalBalance = derivedPtr->getBalance();
            cout << "old base salary : $" << OriginalBalance << endl;
            derivedPtr->adjustBalance(1.10 * OriginalBalance);
            cout << "After adding 10% the balance comes out to." << derivedPtr->getBalance() << endl;
        }
        cout << "earned $" << AccountPtr->() << "\n\n";
    }
    for (const Account* AccountPtr : Account){
        cout << "deleting objects of" << typeid(*AccountPtr).name() << endl;
        delete AccountPtr;
    }
