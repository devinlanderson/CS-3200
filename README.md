# CS-3200
CREATE TABLE Team(
  team_id int IDENTITY(1,1) PRIMARY KEY,
  team_name varchar(255) NOT NULL,
);
GO
CREATE TABLE Player(
  player_id int IDENTITY(1,1) PRIMARY KEY,
  first_name varchar(255) NOT NULL,
  last_name varchar(255) NOT NULL,
  age int NOT NULL,
  team_id int FOREIGN KEY REFERENCES Team(team_id)
);
GO
 
Create TABLE Positions(
  position_id int IDENTITY(1,1) PRIMARY KEY,
  position_name varchar(255) NOT NULL
);
GO
 
CREATE TABLE PlayerPosition(
  player_position_id int IDENTITY(1,1) PRIMARY KEY,
  player_id int FOREIGN KEY REFERENCES Player(player_id),
  position_id int FOREIGN KEY REFERENCES Positions(position_id)
);
GO
CREATE TABLE Game(
  game_id int IDENTITY (1,1) PRIMARY KEY,
  game_date datetime NOT NULL,
  game_location varchar(255) NOT NULL,
);
GO
CREATE TABLE GameResult(
  game_result_ID int IDENTITY (1,1) PRIMARY KEY,
  runs varchar(255) NOT NULL,
  runs_allowed varchar (255) NOT NULL,
  game_id int FOREIGN KEY REFERENCES Game(game_id),
  team_id int FOREIGN KEY REFERENCES Team(team_id),
);
GO
CREATE TABLE BattingStats(
  batter_id int IDENTITY (1,1) PRIMARY KEY,
  BA float(3) NOT NULL,
  OPS float (3) NOT NULL,
  HITS int NOT NULL,
  HR int NOT NULL,
  player_id int FOREIGN KEY REFERENCES Player(player_id)
);
GO
CREATE TABLE PitchingStats(
  pitcher_id int IDENTITY (1,1) PRIMARY KEY,
  ERA float (2) NOT NULL,
  WINS int NOT NULL,
  player_id int FOREIGN KEY REFERENCES Player(player_id),
);
GO
 
Insert
 
INSERT INTO Team (team_name) VALUES
('Orioles'),
('RedSox'),
('WhiteSox'),
('Guardians'),
('Tigers'),
('Astros'),
('Royals'),
('Angels'),
('Twins'),
('Yankees'),
('Athletics'),
('Mariners'),
('Rays'),
('Rangers'),
('BlueJays'),
('Diamondbacks'),
('Braves'),
('Cubs'),
('Reds'),
('Rockies'),
('Dodgers'),
('Marlins'),
('Brewers'),
('Mets'),
('Phillies'),
('Pirates'),
('Cardinals'),
('Padres'),
('Giants'),
('Nationals');
GO
 
INSERT INTO Player (first_name, last_name, age, team_id) VALUES
('Bryce', 'Harper', 29, 25),
('Nolan', 'Arenado', 31, 27),
('Jose', 'Altuve', 32, 6),
('Freddie', 'Freeman', 32, 21),
('JT', 'Realmuto', 31, 25),
('Dylan', 'Cease', 26, 3),
('Justin', 'Verlander', 39, 6),
('Sandy', 'Alcantara', 26, 22),
('Shane', 'Bieber', 27, 4),
('Daniel', 'Bard', 37, 20);
GO
INSERT INTO Positions (position_name) VALUES
('Pitcher'),
('Catcher'),
('FirstBase'),
('SecondBase'),
('ThirdBase'),
('ShortStop'),
('LeftField'),
('CenterField'),
('RightField'),
('DesignatedHitter');
GO
 
INSERT INTO PlayerPosition (player_id, position_id) VALUES
(1,10),
(2,5),
(3,4),
(4,3),
(5,2),
(6,1),
(7,1),
(8,1),
(9,1),
(10,1);
GO
INSERT INTO PitchingStats (player_id, ERA, WINS) VALUES
(7, 1.75, 18),
(6, 2.20, 14),
(8, 2.28, 14),
(9, 2.88, 13),
(10, 1.79, 6);
GO
INSERT INTO BattingStats (player_id, BA, HR, OPS) VALUES
(1, .286, 18, .877),
(2, .293, 30, .891),
(3, .300, 28, .921),
(4, .325, 21, .918),
(4, .276, 22, .820);
GO
Queries
SELECT player_id
FROM Player
WHERE team_id = 25
  (SELECT MAX(BA)
  FROM BattingStats);
GO
 
SELECT BA FROM BattingStats
ORDER BY BA DESC;
GO
SELECT BA FROM BattingStats
HAVING min(BA)> .300;
GO
SELECT ERA FROM PitchingStats
WHERE ERA < 3;
GO
SELECT COUNT(position_id) FROM PlayerPosition
WHERE position_id = 6;
GO
SELECT player_id FROM PlayerPosition
WHERE position_id = 2;
GO
SELECT position_id FROM PlayerPosition
Where position_id IN (
  SELECT team_id
  FROM Team
  WHERE team_id = 21
);
GO
 
SELECT Wins FROM PitchingStats
WHERE Wins > 0;
GO
SELECT player_id FROM Player
WHERE team_id = 12
  (SELECT MAX(HR)
  FROM BattingStats);
GO
 
SELECT BA FROM BattingStats
WHERE player_id = 1;
GO
 
Triggers
 
DROP TRIGGER IF EXISTS team_insert;
GO;
CREATE TRIGGER team_insert ON Team INSTEAD OF INSERT
AS BEGIN
  DECLARE @team_id CHAR(128);
  DECLARE @team_name CHAR(128);
  SELECT @team_id = team_id, @team_name = team_name FROM INSERTED;
  INSERT INTO Team (team_id, team_name) VALUES (@team_id, @team_name);
END;
DROP TRIGGER IF EXISTS player_delete;
GO
CREATE TRIGGER player_delete ON Player INSTEAD OF DELETE
AS BEGIN
  DECLARE @player_id INT;
  DECLARE @count INT;
  SELECT @player_id = player_id FROM DELETEd;
  SELECT @count = COUNT(*) FROM Player WHERE player_id = @player_id;
  IF @count = 0
      DELETE FROM Player WHERE player_id = @player_id;
END;

VIEWS
1. CREATE VIEW Player_Age (Player_ID, Age)
AS
SELECT Player_ID, AGE
FROM Player
WHERE AGE>=30
2. CREATE VIEW Pitcher_ERA (Pitcher_ID, ERA)
AS 
SELECT Pitcher_ID, ERA
From Pitching_Stats
WHERE ERA<=2
3. CREATE VIEW Home_Runs (Batter_ID, HR)
AS
SELECT Batter_ID, HR
FROM Batting_Stats
WHERE HR>=25

PROCEDURES
1. CREATE PROCEDURE SelectAllPitchers
AS
SELECT Player_ID, Position_Name
FROM Player_Position
WHERE Position_Name=Pitcher
GO;
2. CREATE PROCEDURE Infielders
AS
SELECT Player_ID, Position_ID
FROM Player_Position
WHERE Position_ID=3 OR 4 OR 5 OR 6
GO;
3. CREATE PROCEDURE Phillies
AS
SELECT Player_ID, Team_ID
WHERE Team_ID=25
GO;
