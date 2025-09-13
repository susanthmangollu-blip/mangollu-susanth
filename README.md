-- Table: Dining
CREATE TABLE Dining (
  id SERIAL PRIMARY KEY,
  location VARCHAR(100),
  menu TEXT,
  hours VARCHAR(50),
  dietary_options TEXT
);

-- Table: Library
CREATE TABLE Library (
  id SERIAL PRIMARY KEY,
  book_title VARCHAR(255),
  author VARCHAR(255),
  availability BOOLEAN,
  location VARCHAR(100)
);
