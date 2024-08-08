# Football Database

This project is a database schema designed to manage school football teams, including players, parents/carers, age groups, coaches, and teams.

## Database Schema

The database consists of the following tables:

- `player`: Stores information about players.
- `parent_carer`: Stores information about parents and carers.
- `player_parent_carer`: Link table that stores the relationships between players and their parents/carers.
- `age_group`: Stores different age groups for teams.
- `coach`: Stores information about coaches.
- `team`: Stores information about teams.
- `team_coach`: Link table that stores the relationships between teams and their coaches.
- `player_team`: Link table that stores the relationships between players and their teams.
  
### Create Tables
```sql
CREATE TABLE player (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name VARCHAR(50)
);

CREATE TABLE parent_carer (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name VARCHAR(50)
);

CREATE TABLE player_parent_carer (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    player_id INT,
    parent_carer_id INT,
    FOREIGN KEY (player_id) REFERENCES player(id),
    FOREIGN KEY (parent_carer_id) REFERENCES parent_carer(id)
);

CREATE TABLE age_group (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    age VARCHAR(50)
);

CREATE TABLE coach (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name VARCHAR(50)
);

CREATE TABLE team (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name VARCHAR(50),
    age_group_id INT,
    FOREIGN KEY (age_group_id) REFERENCES age_group(id)
);

CREATE TABLE team_coach (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    team_id INT,
    coach_id INT,
    FOREIGN KEY (team_id) REFERENCES team(id),
    FOREIGN KEY (coach_id) REFERENCES coach(id)
);

CREATE TABLE player_team (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    player_id INT,
    team_id INT,
    FOREIGN KEY (player_id) REFERENCES player(id),
    FOREIGN KEY (team_id) REFERENCES team(id)
);
```

### Sample Data

The following sample data can be used to populate the database:

```sql
INSERT INTO player (name) VALUES 
('Lil Bill'), ('Tiny Tim'), ('Small Sal'), ('Mighty Meg'),
('Brill Beth'), ('Awesome Avril'), ('Super Sanna'), ('Delightful Dave'),
('Jumping Jim'), ('Skipping Sally'), ('Boring Boris'), ('Cleethorpes Kate'),
('Billy No-mates');

INSERT INTO parent_carer (name) VALUES 
('John'), ('Julie'), ('Jackie'), ('Jack'), ('Jeremiah');

INSERT INTO player_parent_carer (player_id, parent_carer_id) VALUES 
(1, 1), (1, 2), (2, 1), (2, 2), (3, 3), (4, 5), (5, 4),
(6, 3), (7, 5), (7, 4), (8, 2), (9, 3), (10, 1), (10, 2), (11, 3), (12, 5);

INSERT INTO age_group (age) VALUES 
('10-12'), ('12-14'), ('14-16');

INSERT INTO coach (name) VALUES 
('Coach Carter'), ('Gareth Southgate'), ('Coachy McCoachface');

INSERT INTO team (name, age_group_id) VALUES 
('Team 1', 1), ('Team 2', 2), ('Team 3', 3);

INSERT INTO team_coach (team_id, coach_id) VALUES 
(1, 3), (2, 1), (3, 2), (3, 3);

INSERT INTO player_team (player_id, team_id) VALUES 
(1, 1), (2, 1), (3, 1), (4, 1),
(5, 2), (6, 2), (7, 2), (8, 2),
(9, 3), (10, 3), (11, 3), (12, 3);
```

### Tests

Retrieve all data
```sql
SELECT * FROM player;
SELECT * FROM parent_carer;
SELECT * FROM player_parent_carer;
SELECT * FROM age_group;
SELECT * FROM coach;
SELECT * FROM team;
SELECT * FROM team_coach;
SELECT * FROM player_team;
```

All players from Team 1
```sql
SELECT * FROM player
JOIN player_team ON player.id = player_team.player_id
JOIN team ON player_team.team_id = team.id
WHERE team.name = 'Team 1';
```

All players with no team
```sql
SELECT * FROM player
LEFT JOIN player_team ON player.id = player_team.player_id
LEFT JOIN team ON player_team.team_id = team.id
WHERE team.name IS NULL;
```


