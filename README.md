# pgAdminHW
sql basics

--  Tablo Oluşturma
--------------------
create table developers (
	developers_id serial primary key,
	
	company_name varchar not null,
	
	country varchar,
	
	founded_year int
);

create table games (
	game_id serial primary key,
	title varchar not null,
	price decimal(10, 2),
	release_date date,
	developer_id int,
	rates decimal(3, 1),

-- Foreign Key Tanımlama (One-to-Many)
---------------------------------------
constraint fk_developer foreign key(developer_id) references developers(developers_id) on delete set null

);

create table genre (
	genre_id serial primary key,
	genre_name varchar not null,
	genre_description text
);

create table games_genres (
	games_genres_id serial primary key,
	games_id int,
	genres_id int,

-- Foreign Key Tanımlama (Many-to-Many)
----------------------------------------
constraint fk_game
        foreign key(games_id) references games(game_id) on delete cascade,

constraint fk_genre
        foreign key(genres_id) references genre(genre_id) on delete cascade

);



--  Veri Ekleme 
----------------
insert into developers (company_name, country, founded_year) values 
('Sea Limited', 'Singapur', 2009),
('Sony', 'Japonya', 1946),
('Bethesda', 'Amerika', 1986),
('Rockstar Games', 'Amerika', 1998),
('CD Projekt Red', 'Polonya', 2002);

insert into games (title, price, release_date, developer_id, rates) values
('Fifa Online 3', 9.99, '2012-12-18', 1, 8.2),
('Grand Theft Auto: San Andreas', 7.99, '2004-11-26', 4, 9.4),
('Starfield', 69.99, '2023-09-06', 3, 6),              
('The Elder Scrolls V: Skyrim', 39.99, '2011-11-11', 3, 9.3),
('Red Dead Redemption 2', 19.99, '2018-10-26', 4, 9.4),   
('League of Legends', 19.99, '2009-10-27', 1, 8.6),
('Cyberpunk 2077', 59.99, '2020-12-10', 5, 9),  
('The Last of Us Part I', 59.99, '2021-09-02', 2, 9.8),
('The Witcher 3: Wild Hunt', 39.99, '2015-05-18', 5, 10),
('God of War', 49.99, '2018-04-20', 2, 10,)
        
insert into genre (genre_name, genre_description) values
('RPG', 'Role Playing Game'),
('Open World', 'Açık Dünya'),
('Moba', 'Çevrimiçi Çok Oyunculu'),
('FPS', 'Birinci Şahıs Nişancı'),
('Action', 'Aksiyon'),
('Sports', 'Spor');


insert into games_genres (games_id, genres_id) values
--Fifa Online 3 (Spor)
(1,6),
--Grand Theft Auto: San Andreas (Aksiyon)
(2,5),
--Starfield (Aksiyon RPG),       
(3,1), (3,5),       
--The Elder Scrolls V: Skyrim (Aksiyon, RPG, Open World)
(4,1), (4,2), (4,5),
--Red Dead Redemption 2 (Aksiyon)
(5,5),
--League of Legends (Moba)
(6,3),
--Cyberpunk 2077 (Aksiyon)
(7,5),
--The Last of Us Part I (Aksiyon)
(8,5),
--The Witcher 3: Wild Hunt (Aksiyon, RPG)
(9,1), (9,5),
--God of War (Aksiyon)
(10,5)


--  Güncelleme 
---------------
update genres set price = '9,99' where game_id = 1
update genres set genres_description = 'Çevrimiçi Çok Oyunculu' where genres_id = 3
update games set price = '7,99' where game_id = 2
update genres set genres_description = 'Birinci Şahıs Nişancı' where genres_id = 4

select
    title,
    price AS "New Price",
    (price / 0.90) AS "Previous Price",
    ((price / 0.90) - price) AS "Discount"
from games;


--  Raporlama 
--------------

-- 1.Tüm Oyunlar Listesi: Oyunun adı, Fiyatı ve Geliştirici Firmanın Adını yan yana getirin (JOIN kullanın).

select title, price, developers.company_name from games inner join developers on games.developer_id = developers.developers_id 

-- 2.Kategori Filtresi: Sadece "RPG" türündeki oyunların adını ve puanını listeleyin (Many-to-Many JOIN).

select title, rates from games 
join games_genres on games.game_id = games_genres.games_id
join genre on games_genres.genres_id = genre.genre_id
where genre.genre_name = 'RPG';


-- 3.Fiyat Analizi: Fiyatı 500 TL üzerinde olan oyunları pahalıdan ucuza doğru sıralayın.

select * from games where price > 12 order by price desc;

-- 4.Arama: İçinde "War" kelimesi geçen oyunları bulun (LIKE).

select * from games where title like '%The%';
