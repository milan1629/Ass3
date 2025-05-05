using System;

namespace BankApp
{
    // ============================================
    // Base class: Client
    // Represents a general bank client
    // ============================================
    internal class Client
    {
        // Unique ID of the client
        public string ID { get; set; }

        // Client's full name
        public string Name { get; set; }

        // Constructor to initialize client ID and Name
        public Client(string strID, string name)
        {
            ID = strID;
            Name = name;
        }

        // Virtual method to set client ID (can be overridden in derived classes)
        public virtual void SetID(string strID)
        {
            ID = strID;
        }
    }

    // ============================================
    // Derived class: BankAcc (Bank Account)
    // Inherits from Client
    // ============================================
    internal class BankAcc : Client
    {
        // Property to store the current balance in the account
        public decimal Balance { get; private set; }

        // Property to store the date the account was opened
        public DateTime OpeningDate { get; private set; }

        // Constructor to create a new BankAcc object
        // Calls the base class constructor to initialize client details
        public BankAcc(string strID, string name, decimal initialBalance)
            : base(strID, name)  // Call base constructor
        {
            // Initial balance must not be negative
            if (initialBalance < 0)
                throw new ArgumentException("Initial balance cannot be negative.");

            Balance = initialBalance;            // Set starting balance
            OpeningDate = DateTime.Now;          // Automatically set opening date to now
        }

        // ============================================
        // Override the SetID method from Client class
        // Add prefix "CL-" if not already present
        // ============================================
        public override void SetID(string strID)
        {
            if (!strID.StartsWith("CL-"))
            {
                strID = "CL-" + strID;
            }

            // Call the base class SetID method with modified ID
            base.SetID(strID);
        }

        // ============================================
        // Method to deposit money into the account
        // Amount must be greater than 0
        // Returns true if successful
        // ============================================
        public bool Deposit(decimal amount)
        {
            if (amount <= 0)
                return false;

            Balance += amount;
            return true;
        }

        // ============================================
        // Method to withdraw money from the account
        // Amount must be > 0 and less than or equal to balance
        // Returns true if successful
        // ============================================
        public bool Withdraw(decimal amount)
        {
            if (amount <= 0 || amount > Balance)
                return false;

            Balance -= amount;
            return true;
        }

        // ============================================
        // Optional Transfer Method (not required)
        // Demonstrates how to transfer between accounts
        // ============================================
        public void Transfer(BankAcc targetAccount, decimal amount)
        {
            // Withdraw from this account and deposit into target
            if (this.Withdraw(amount))
            {
                targetAccount.Deposit(amount);
            }
        }
    }
}
