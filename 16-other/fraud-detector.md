# Amazon Fraud Detector

- Is a fully managed fraud detection service
- Allows us to look at various historical trends and other related data and identify any potential fraud as it related to certain only activities such as new account creation, payments or guest checkouts
- We upload some historical data and we have to chose a model type:
    - Online Fraud: needs little historical data
    - Transaction Fraud: idean when we have a transactional history for a customer, identifies suspect payments
    - Account Takeover: used to identify phishing or another social based attack
- Events are scored, based on which we can create rules/decisions to react to events according to our business activity