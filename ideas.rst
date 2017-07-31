Ideas for Bite-Sized Conversational Bots
===========================================

Invitation Decliner
---------------------

Design a chatbot that declines invitations. 

Callflow:

- The user extends some kind of an invitation. "Hi, I'm calling to invite you to my birthday party." "Would you like to join me for a run?" "Do you want to go out for lunch?"
- The chatbot recognizes the invitation and declines it.

Advanced feature:

- The reason for declining the invitation is chosen at random. (You can use the Math.random() function in JavaScript.)

Restaurant Reservation Taker
------------------------------------------

Design a chatbot that takes restaurant reservations.  The user will be instructed to place a reservation for 7pm tonight for 4 people.

Callflow:

- The robotic restaurant employee greets the customer.
- The user says something like: Hi, can I make a reservation?" "I'd like to make a reservation" "Could I make a reservation for tonight?"
- The bot asks:
  - how many people the reservation is for (unless already specified)
  - what time (unless already specified)
-	The bot confirms the reservation.

Appology acceptor 
-------------------------

Design a chatbot that accepts apologies.

The user will be given the following instructions:

"You messed up. Earlier today, you were supposed to meet your friend Lara for lunch, but you completely forgot about it. Lara waited for you and then had to eat alone. Call her to apologize."

Callflow:

-	The robo-friend picks up the phone: "Hello?"
-	If the user apologizes immediately: "Oh, that's OK, don't worry about it. Let's reschedule our lunch for next week."
-	If the user just says hello, respond with something like "Oh, hi. So what happened?"
  - If the user apologizes then, accept the apology.
  - If there is no apology, say something like: "Hmm OK. I see."
