# Test Task: REST API for Slot Games
## Task Overview:
You are tasked with creating a simple REST API that mimics the core functionalities of a slot game system. The API should allow users to:

- Register and log in
- Deposit and withdraw credits
- Spin the slot machine
- Check game history
- Retrieve the current credit balance

## Functional Requirements:
### 1. User Management.

- Registration: Allow a user to register using their email and password.
- Login: Provide authentication with token-based authorization (e.g., JWT).
- Get User Profile: Return basic user info and credit balance.
### 2. Wallet Management

- Deposit Credits: Add credits to a user's balance.
- Withdraw Credits: Subtract credits from a user’s balance.
### 3. Game Logic: Slot Machine Spin

- Spin: A user can spin the slot machine by betting a specific amount of credits. If they win, they should be awarded credits based on a simple algorithm. 3 symbols, which are a random numbers from 1 to 9. For example:[1,2,3], [7,7,7], [3,5,9]. In those combinations each number is a random value.

	- Inputs:
		- Bet amount (must be within the user’s available balance).
	- Outputs:
		- Updated credit balance.
		- Spin result (e.g., winning combination or loss). 
	- Payout Calculation: Implement a basic payout table where winning combinations result in various payouts.
		- Example:
			- Three identical symbols: 10x bet (7,7,7).
			- Two identical symbols: 2x bet (7,7,3).
			- No identical symbols: loss of bet (7,8,9).
### 4. Game History

- List Game History: Show a list of all spins, including the result, bet, and any winnings for the user.
## Endpoints Overview:
### 1. User Management.

- POST /api/register: Register a new user.
- POST /api/login: Log in and receive an authentication token.
- GET /api/profile: Retrieve the user profile and credit balance (authorization required).
### 2. Wallet Management

- POST /api/wallet/deposit: Deposit credits to the user's balance (authorization required).
- POST /api/wallet/withdraw: Withdraw credits from the user's balance (authorization required).
### 3. Game Logic

- POST /api/slot/spin: Spin the slot machine, place a bet, and get the result (authorization required).
### 4. Game History

- GET /api/slot/history: Retrieve a list of the user's past spins (authorization required).
## Technical Requirements:
- Use Go for the backend development.
- Implement RESTful APIs.
- Authentication: Use JWT for securing endpoints.
- Use PostgreSQL (or any preferred database) to persist user data, transactions, and game history.
- Write unit tests for the key components of the system.
- Document the API using Swagger.
#### Bonus Points:
- Dockerize the application for easy setup.
- Add rate-limiting to prevent abuse (e.g., excessive API calls for spinning).
- Implement a retry mechanism for the spin function in case of failure.
## Deliverables:
- Codebase (with instructions on how to run it).
- Postman collection or Swagger documentation for API testing.
- A readme file explaining your approach and assumptions.