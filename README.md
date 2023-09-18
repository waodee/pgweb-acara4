-- untuk menghitung jumlah data
SELECT count(*) FROM persons p

-- untuk menampilkan data
select * from persons p

-- untuk menampilkan beberapa field 
select firstnames as nama_depan, lastname as nama_belakang, birthdate as tanggal_lahir, extract(year from age (birthdate)) as umur_tahun, extract(month from age (birthdate)) as umur_bulan, extract(day from age (birthdate)) as umur_hari from persons p

-- untuk menampilkan berapa jumlah orang dengan umur kurang dari 30 tahun
select count(extract(year from age (birthdate))) as umur_tahun from persons p 
where extract(year from age (birthdate)) < 30

-- untuk menampilkan orang dengan umur antara 25 - 35 tahun 
select count(extract(year from age (birthdate))) as umur_tahun from persons p
where extract(year from age (birthdate)) between 25 and 35

-- join persons, gender, maritalstatus, dan cities
select p.*, g.name as jenis_kelamin, m."name" as status_perkawinan, c."name" as kota_domisili
from persons p 
join genders g on p.gender = g.id 
join maritalstatus m on p.maritalstatus_id = m.id
join cities c on p.city_id = c.id

-- berapa orang yang kota domisili sama dengan kota kelahiran
select concat(firstname, ' ', lastname), p.hometown, c."name" as kota_domisili
from persons p
join cities c on p.city_id = c.id
where p.hometown = c."name"

-- orang yang tidak memiliki kartu kredit
select count (*) from persons p
where creditcardtype_id is not null

-- orang yang memiliki kartu kredit
select count (*) from persons p
where P.creditcardstype_id is null

-- jumlah laki--laki
select count (*) from persons p
where  gender = 1

-- jumlah laki-laki belum menikah
select count(*) from persons p 
where gender = 1 and maritalstatus_id = 1

-- jumlah perempuan belum menikah
select count(*) from persons p 
where gender = 2 and maritalstatus_id = 1

-- jumlah janda dengan umur > 50 tahun
select count(*) from persons p 
where gender = 2 and 
(maritalstatus_id = 3 or maritalstatus_id = 4) and 
extract(year from age (birthdate)) > 50

-- jumlah kepala desa
select count(*) from persons p
where jobtitle_id = 25

-- jumlah orang yang belum/tidak bekerja
select count(*) from persons p
where jobtitle_id = 5

-- jumlah guru hampir pensiun (> 55 tahun)
select count(*) from persons p 
where jobtitle_id = 15 and 
extract(year from age (birthdate)) > 55

-- untuk membuat geom point
update persons set geom = st_setsrid(st_makepoint(longitude, latitude), 4326)
