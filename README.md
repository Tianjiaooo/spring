flowchart TD
  A[HCL Leap Form: User initiates bill payment] --> B[Redirect to ShopCart]
  B --> C[Checkout page shows payment details (amount)]
  C --> D[Redirect to Xplor payment page]
  D --> E[Xplor processes the payment]
  E --> F[Return / Redirect back to HCL Leap Form]
  F --> G[Call backend service to save record to Amanda]
  G --> H[Call email service to send confirmation email]
Hi, quick question: how is the payment status in Amanda updated for this flow? Is it pushed back from Xplor, or do we update it ourselves after verifying the payment?”
