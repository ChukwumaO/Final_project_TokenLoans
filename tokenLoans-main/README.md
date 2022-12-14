# TokenLoans
## Decentralized Finance for Everyone

<p align='center'> <img src='images/decentralized_finance.jpg'></p>

TokenLoans is a solidity contract that allows borrowers to fundraise Ether for a project. Investors can buy the tokens produced by the contract (ERC20 tokens) and they get paid back the principal and interest in equal monthly installments. The borrower selects the terms of the contract and investors decide whether to invests or not. 

The borrower sells its tokens at a discount and has to buy them back at parity. 

### Sell Tokens

In this example the borrower sells its tokens at a ratio of 900 to 1000 i.e. 900 Ether gives the investor 1000 tokens. 

<p align='left'> <img src='images/discounted_rate.JPG'width="500"></p>

### Buy Back the Tokens

The borrower has to buy back the tokens at parity i.e. 1 token gives back 1 Ether. The tokens produced by the contract have the same number of decimals (18) as Ether. 
<p align='left'> <img src='images/parity_rate.JPG'width="500"></p>

The difference between the sell and buy price is the interest the investor makes. In this example the investors get an interest of 11.11%. The user has to pay in equal monthly installments. 

## What the Contract Cannot Do

* Force payment
* Schedule a payment 

Every solidity contract has these limitations. The Ethereum Virtual Machine does not offer a scheduling service and it cannot extract money from a private wallet. The contract, however, does provide two public variables that can be used to determine the health of the contract: percentage_paid_to_investors and contract_ended_successfully (this boolean turns true if the investors were paid within the timeframe stated by the borrower).

## How to Set Up the Contract

The contract has 5 constructors (as seen in Remix IDE):

<p align='left'> <img src='images/constructors.JPG' width="500"></p>

1) Fundraising goal in wei: If the borrower wants 1 Ether they will have to put 1 followed by 18 zeros. 
2) Discounted price for 1000TKL: How much will an investor pay for 1000 of the borrower's tokens? This will define the exchange rate the borrower is selling its tokens for. 1000 means that the investors get zero interest. We suggest a number between 950 (interest of 5.26%) and 800 (interest of 25%).
3) Maturity in months: How many equal monthly payments does the borrower want to make?
4) Number of fundraising days: The borrower has to define a period for the fundraising. If the time ends and the goal hasn't been reached investors can pull their money out. 
5) Grace period in months: number of months before the borrower has to start making payments. 

The wallet that deploys the contract becomes the owner.

## Main Functions

#### Buy TKL with Wei
<p align='left'> <img src='images/buy_tkl.JPG' width="200"></p>
<p align='left'> <img src='images/value.JPG' width="500"></p>

This function is public and can only be called before the fundraising ends. The fundraising ends when the fundraising goal is reached. In conjunction with the value interface in Remix the investor uses this button to buy tokens, there is a check in the function that stops the investor from buying more tokens than allowed by the fundraising goal. The investor can check the number of tokens bought by clicking the balanceOfSender button. 

<p align='left'> <img src='images/balance_of_sender.JPG' width="200"></p>

#### Make Monthly Payment

<p align='left'> <img src='images/make_monthly_payment.JPG' width="200"></p>

Only the owner can call this function after the fundraising ends. It takes the number of tokens sold and divides that between the number of monthly payments. It sends out a payment to investors proportional to the number of tokens they bought. It does not take into account possible early payments made by the borrower. 

#### Send Payment to Investors

<p align='left'> <img src='images/send_payment.JPG' width="500"></p>

Only the owner can call this function after the fundraising ends. It divides up the amount entered in the interface among investors proportional to the tokens they bought. It is not possible for the borrower to pick one particular investor to receive the payment. The contract does not penalize early payment. 

#### Withdraw Funds

<p align='left'> <img src='images/withdraw_funds.JPG' width="500"></p>

Only the owner can call this function after the fundraising ends. The funds raised live in the contract, not in the owner's wallet. The owner has to use this function to withdraw funds.

#### Investor Refund Principal

<p align='left'> <img src='images/investor_refund.JPG' width="200"></p>

Anyone can call this function if two conditions are true: the fundraising time has ended and the fundraising goal hasn't been reached. Investors get refunded without interest. 

#### Owner Refund Principal

<p align='left'> <img src='images/owner_refund.JPG' width="200"></p>

Only the owner can call this fuction before the fundraising ends. Investors get refunded without interest.

#### Deposit

<p align='left'> <img src='images/deposit.JPG' width="200"></p>

Only the owner can call this function after the fundraising ends. Investors get paid from the funds that are in the contract, the borrower has to deposit enought funds to make the payments. 

## Main Public Variables

#### Contract Ended Successfully

<p align='left'> <img src='images/contract_ended.jpg' width="600"></p>

The default value of this boolean is false. It turns true if all investors get paid within the timeframe specified by the borrower. 

#### Fundraising Ended

<p align='left'> <img src='images/fundraising_ended.jpg' width="600"></p>

The default value of this boolean is false. It turns true when the fundraising goal is reached. 

#### Percentage Paid to Investors

<p align='left'> <img src='images/percentage_paid.JPG' width="200"></p>

Percentage of total tokens sold that have been refunded. This is usually 1% off due to lack of float type in solidity. 

#### Time

<p align='left'> <img src='images/time.JPG' width="200"></p>

The contract timestamps three events: deployment of the contract, fundraising ends, payment to investors is complete. It is possible that one of the last two benchmarks is not met, in that case the value is zero. Like in all solidity contracts time is recorded as number of seconds since Jan 1st 1970.

#### Total Contract Funds and Total Supply

<p align='left'> <img src='images/total_supply.JPG' width="300"></p>

totalContractFundsinWei tells the user the total funds in the contract. Total supply is the number of tokens that are outstanding in the hands of investors. This example is from when a fundraising ended and no funds have been withdrawn, the borrower fundraised 250 ether, notice that the number of tokens is higher, the difference is the interest. 

#### Outstanding Tokens Minus Contract Funds

<p align='left'> <img src='images/difference.JPG' width="300"></p>

This variable tells the user the difference between the outstanding tokens and the contract funds. It helps the borrower know how much do they have to deposit in the contract to make the investors whole. 

#### Constructors and Owner

<p align='left'> <img src='images/maturity_in_months.JPG' width="200"></p>

<p align='left'> <img src='images/fundraising_goal.JPG' width="300"></p>

<p align='left'> <img src='images/owner.JPG' width="300"></p>

All the variables that were used as constructors and the owner of the contract are public. 










