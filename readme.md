1. What was the most challenging part of designing this API?
The hardest part was designing consistent error handling and anticipating edge cases across all endpoints.

2. Which endpoint was the hardest to design and why?
POST /api/loans was most complex because it required validating members, books, and availability simultaneously

3. How did you decide which operations should be nested resources vs. top-level resources?
I nested operations that only made sense in relation to another resource, like loans under members.

4. If you had to add a "Reviews" feature (users can review books), how would you design those endpoints?
Reviews would be nested under books with endpoints like /api/books/:id/reviews for creation and retrieval.

5. What is one thing you would do differently if you started over?
I would establish a standard error response format upfront to ensure uniformity across the API.