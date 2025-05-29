# BookMyShow
BookMyShow Schema


**Table Schema**
UserTable-userid,userEmail,password,phoneNumber,city,pincode

MovieTable-MovieId,movieName,releaseDate,cast,Duration

TheatreTable-theatreId,Location,theatreName,State

ScreenTable-screen_id,Sceen_type,theatre_id

showtable-ShowId,showTime,mpviId,showDate

seatsTable-seatId,seatNumber,screenId,rowNumber

BookingsTable-BookingId,showId,user_id,mobileNumber,price,bookingTime


**Schema cardinality**
user-Bookings(one to many)
theatre- screens(one to many)
screen-shows(one to many)
show-user(one to one)

P1: SQL Queries for table creation and insertion of values: (P1 task)

Movies Table
CREATE TABLE Movies ( movie_id INT PRIMARY KEY AUTO_INCREMENT, title VARCHAR(255) NOT NULL, language VARCHAR(50), genre VARCHAR(50), duration_min INT );

INSERT INTO Movies (title, language, genre, duration_min) VALUES
('Inception (2010)', 'English', 'Sci-Fi', 148),
('RRR (2022)', 'Telugu', 'Action', 182),
('Parasite (2019)', 'Korean', 'Thriller', 132);

Theatres Table
CREATE TABLE Theatres ( theatre_id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(255) NOT NULL, location VARCHAR(255) NOT NULL );

INSERT INTO Theatres (name, location) VALUES
('Cinepolis: Banjara Hills', 'Hyderabad'),
('Carnival: Koramangala', 'Bangalore');

Screenstable
CREATE TABLE Screens ( screen_id INT PRIMARY KEY AUTO_INCREMENT, theatre_id INT NOT NULL, name VARCHAR(50) NOT NULL, FOREIGN KEY (theatre_id) REFERENCES Theatres(theatre_id) );

INSERT INTO Screens (theatre_id, name) VALUES
(1, 'Screen A - 4DX'),
(1, 'Screen B'),
(2, 'Screen 1 - Dolby Vision'),
(2, 'Screen 2');

ShowsTable
CREATE TABLE Shows ( show_id INT PRIMARY KEY AUTO_INCREMENT, movie_id INT NOT NULL, screen_id INT NOT NULL, show_date DATE NOT NULL, show_time TIME NOT NULL, FOREIGN KEY (movie_id) REFERENCES Movies(movie_id), FOREIGN KEY (screen_id) REFERENCES Screens(screen_id) );

INSERT INTO Shows (movie_id, screen_id, show_date, show_time) VALUES
(1, 1, '2024-07-01', '09:30:00'), -- Inception
(1, 2, '2024-07-01', '14:15:00'),
(2, 3, '2024-07-01', '11:00:00'), -- RRR
(3, 4, '2024-07-01', '19:45:00'); -- Parasite

SeatsTable
CREATE TABLE Seats ( seat_id INT PRIMARY KEY AUTO_INCREMENT, screen_id INT NOT NULL, row_number INT NOT NULL, seat_number INT NOT NULL, FOREIGN KEY (screen_id) REFERENCES Screens(screen_id) );

INSERT INTO Seats (screen_id, row_number, seat_number) VALUES (1, 1, 1), (1, 1, 2), (1, 1, 3), ..., (1, 10, 10), -- Seats in Row 1 (2, 1, 1), (2, 1, 2), (2, 1, 3), ..., (2, 10, 10); 

Bookings table
CREATE TABLE Bookings (
    booking_id INT PRIMARY KEY AUTO_INCREMENT,
    show_id INT NOT NULL,
    user_id INT,
    num_tickets INT NOT NULL,
    total_price DECIMAL(10,2) NOT NULL,
    booking_time DATETIME NOT NULL,
    FOREIGN KEY (show_id) REFERENCES Shows(show_id)
    -- FOREIGN KEY (user_id) REFERENCES Users(user_id) -- Uncomment if Users table exists
);

INSERT INTO Bookings (show_id, user_id, num_tickets, total_price, booking_time) VALUES
(1, 5, 1, 200.00, '2024-06-28 08:15:00'),  -- Random user books: Inceptio,n morning show
(4, 8, 4, 1000.00, '2024-06-28 18:30:00'); -- Random user books Parasite night show



Write a query to list down all the shows on a given date at a given theatre along with their respective show timings.

SELECT a.show_time, m.title AS movie_title, t.name AS theatre_name FROM Shows a INNER JOIN Movies m ON a.movie_id = m.movie_id INNER JOIN Screens sc ON a.screen_id = sc.screen_id INNER JOIN Theatres t ON sc.theatre_id = t.theatre_id WHERE a.show_date = CURRENT_DATE AND t.name = 'Cinepolis';







