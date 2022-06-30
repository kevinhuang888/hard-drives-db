/* Kevin Huang
		Jun 29, 2022
    PostgreSQL "Hard Drive Schema" */


/*
	Self-made project. 
  I have decided to investigate hard drives because my parent works in that industry and it's something I wanted to get to know better.
  This system will involve 3 tables: hard drive, media (disks), and head.
  We will create a database to illustrate this system for a hypothetical company to use.
*/


/*
	Hard Drive
*/
CREATE TABLE drives (
  id varchar PRIMARY KEY,
  gas varchar,
  generation integer
);

/*
	Media
  	id - 
    vendor - who built the disk
    is_good - true if there are no degeneracies with the disk
    generation - what gen is the disk 
    drive_id - which drive does the disc belong in  <-- Foreign Key
*/
CREATE TABLE media (
  id varchar PRIMARY KEY, 
  vendor varchar(70),  
  is_good boolean,  
  generation integer,
  drive_id varchar REFERENCES drives(id)
);


/*
	Head
*/
CREATE TABLE heads (
  id varchar PRIMARY KEY,
  vendor varchar(70),
  is_good boolean,
  generation integer,
  drive_id varchar REFERENCES drives(id) UNIQUE
);


/*
	Inserting Sample Data
*/

INSERT INTO drives
VALUES (
  'AA-001',
  'helium',
  1
);

INSERT INTO media
VALUES (
  'MD-001',
  'Seagate',
  false,
  1,
  NULL
);

INSERT INTO media
VALUES (
  'MD-002',
  'Seagate',
  true,
  1,
  'AA-001'
);

INSERT INTO media
VALUES (
  'MD-003',
  'Seagate',
  true,
  1,
  'AA-001'
);

INSERT INTO media
VALUES (
  'MD-004',
  'Seagate',
  true,
  1,
  'AA-001'
);

INSERT INTO media
VALUES (
  'MD-005',
  'SanDisk',
  true,
  2,
  NULL
);

INSERT INTO heads
VALUES (
  'HD-002',
  'HP',
  true,
  1,
  'AA-001'
);

INSERT INTO heads
VALUES (
  'HD-003',
  'HP',
  true,
  1,
  NULL
);

/*
Test of PRIMARY KEY to see if each id will be unique

  INSERT INTO heads
  VALUES (
    'HD-003',
    'HP',
    true,
    1,
    NULL
	);
*/

/*
Test to see if Head and Drive one-to-one relationship holds up
  
INSERT INTO heads
VALUES (
  'HD-004',
  'HP',
  true,
  1,
  'AA-001'
);
*/

/*
	Let's insert some more drives as well as the respective media and heads to make things more interesting!
*/

INSERT INTO drives
VALUES (
  'VH-002',
  'hydrogen',
  2
);

INSERT INTO drives
VALUES (
  'XP-003',
  'helium',
  3
);

INSERT INTO media
VALUES (
  'MD-006',
  'WD',
  true,
  2,
  'VH-002'
);

INSERT INTO media
VALUES (
  'MD-007',
  'WD',
  true,
  2,
  'VH-002'
);


/*
	The company decides to use the extra disk that hasn't been allocated for drive VH-002. We have to update the media table to reflect this.
*/
UPDATE media
SET drive_id = 'VH-002'
WHERE id = 'MD-005';

INSERT INTO heads
VALUES (
  'HD-004',
  'HP',
  true,
  2,
  'VH-002'
);

INSERT INTO heads
VALUES (
  'HD-005',
  'WD',
  true,
  3,
  'XP-003'
);

/*
	So far we have just been relating involving the drives table.
	Let's use a JOIN to see the relationship between the heads table and the media table.
  From there we can see which head will be reading which disks.
*/
SELECT *
FROM heads
JOIN media
ON heads.drive_id = media.drive_id;

/*
	Inner JOIN got rid of all of the NULL heads
  Let's try a LEFT JOIN as well to display all heads!
*/
SELECT *
FROM heads
LEFT JOIN media
ON heads.drive_id = media.drive_id;

/*
	This concludes this exploration. We have successfully created a schema to describe the relationship between hard drives, media, and heads.
  We are also able to populate our tables following the schema rules and can manipulate the data as we choose.
*/
  
  
  