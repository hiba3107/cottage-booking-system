Cottage Booking System

A Java console application for a holiday letting company. Guests search for a cottage against five criteria, reserve it against their email address, and cancel later. Everything persists to a text file between runs.

University coursework, 4300COMP Introduction to Programming, Liverpool John Moores University.

What it does

Guests search on five requirements at once:

Cottage type: Terraced, Detached or Bungalow
Party size, which cannot exceed the cottage's maximum occupancy
Maximum price per night
Sea view, or not
Pets allowed, or not

A menu-driven loop lets the user list every cottage, list only the reserved ones, search, book and cancel. Reservations are keyed to the guest's email address, and the file is written back on every booking, every cancellation and on exit, so nothing is lost if the program is closed.

The part worth reading the code for

The next best match. Rejecting someone because one of their five requirements cannot be met is a bad answer. So when nothing matches exactly, the system scores every cottage and offers the three closest, which the guest can book straight from the list.

The scoring is weighted by how much each requirement actually matters:

Factor	Effect on score
Right cottage type	+50
Occupancy off by one person	-5 per person
Over the guest's budget	penalty scaled to how far over
Under budget	small bonus, capped
Sea view matches / does not	+20 / -10
Pets matches / does not	+20 / -10
Currently unbooked	+10

Type is weighted most heavily because it is the thing guests are least willing to compromise on. Price is penalised proportionally rather than as a hard cut, because being £5 over budget is not the same as being £80 over. Occupancy is scaled by how far out it is rather than being pass or fail.

The candidates are then sorted by score and the top three returned.

Input is never trusted. Every numeric read loops until it gets a valid number rather than crashing on a bad parse. Yes or no questions accept several forms and re-ask otherwise. Email addresses are checked before a booking is accepted. Cancelling a cottage that was never booked is caught and explained rather than silently doing nothing.

Design

Two classes, split by responsibility.

Cottage holds one cottage's data with private fields and getters, so its booked state can only change through book() and cancelBooking() rather than being written to directly from outside. toString() produces the display line.

BookingSystem handles file loading and saving, the menu loop, searching, scoring and the booking workflow.

The design document for this coursework included a nouns and verbs analysis of the specification and a UML class diagram justifying those decisions.

What I would do differently

The cottages are held in a fixed array sized to the eighteen records in the supplied file. That works for the brief but it is the weakest decision in the code: the program cannot read a file of a different length without being edited. An ArrayList would have cost nothing and removed the assumption entirely.

The filename is also hardcoded rather than being a constant or an argument, which is the same kind of problem one level down.

Running it
javac Cottage.java BookingSystem.java
java BookingSystem

The data file needs to be in the working directory. It holds one cottage per line:

cottageNum  type  maxOccupancy  price  hasSeaView  allowPets  email

An unreserved cottage has free in the final field.

Built with

Java. No libraries beyond the standard one, no framework, no build tool.
