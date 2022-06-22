# DB-biblioteca
Script de creación DB biblioteca

create database biblioteca;

create table libros (
	n_serie_libro int not null auto_increment primary key,
    nombre_libro varchar(30) not null,
    escritor varchar(20) not null ,
    unidades int,
    genero varchar(20) not null ,
    descripcion varchar(50) not null);

create table empleados (
	n_empleado int not null auto_increment primary key,
    nombre varchar(20) not null,
    apellido varchar(20) not null ,
    puesto varchar(20) null);

create table cliente (
	dni_cliente int not null auto_increment primary key,
    nombre varchar(20) not null,
    apellido varchar(20) not null ,
    fecha date not null);
    
create table compra (
	id_compra int not null auto_increment primary key,
    dni_cliente int not null,
    n_serie_libro int not null,
    fecha date not null,
    hora time not null,
    n_empleado int not null);

ALTER TABLE compra ADD CONSTRAINT FK_dni_cliente FOREIGN KEY FK_dni_cliente (dni_cliente)
    REFERENCES cliente (dni_cliente);

ALTER TABLE compra ADD CONSTRAINT FK_n_serie_libro FOREIGN KEY FK_n_serie_libro (n_serie_libro)
    REFERENCES libros (n_serie_libro);

ALTER TABLE compra ADD CONSTRAINT FK_n_empleado FOREIGN KEY FK_n_empleado (n_empleado)
    REFERENCES empleados (n_empleado);

create table editorial (
	n_cuit_editorial int not null auto_increment primary key,
    nombre varchar(20) not null,
    escritores varchar(20) not null,
    n_serie_libro int not null);
    
ALTER TABLE editorial ADD CONSTRAINT FK_n_serie_libro_editorial FOREIGN KEY FK_n_serie_libro (n_serie_libro)
    REFERENCES libros (n_serie_libro);

create table libros_empleados (
	n_serie_libro int not null,
    n_empleado int not null,
    biblioteca varchar(10) null,
    primary key (n_serie_libro, n_empleado),
	constraint FK_n_serie_libro_empleados foreign key (n_serie_libro) references libros (n_serie_libro),
	constraint FK_n_empleado_libros foreign key (n_empleado) references empleados (n_empleado));

create table libros_cliente (
	n_serie_libro int not null,
    dni_cliente int not null,
    nombre_cliente varchar(20) not null,
    nombre_libro varchar(30) not null,
    escritor varchar(20) not null,
    primary key (n_serie_libro, dni_cliente),
	constraint FK_n_serie_libro_cliente foreign key (n_serie_libro) references libros (n_serie_libro),
	constraint FK_dni_cliente_libros foreign key (dni_cliente) references cliente (dni_cliente));
    
create table libros_compra (
	n_serie_libro int not null,
	id_compra int not null,
	nombre_libro varchar(30) not null,
	unidades int,
	primary key (n_serie_libro, id_compra),
	constraint FK_n_serie_libro_compra foreign key (n_serie_libro) references libros (n_serie_libro),
	constraint FK_id_compra_libros foreign key (id_compra) references compra (id_compra));

create table libros_editoriales (
	n_serie_libro int not null,
	n_cuit_editorial int not null,
	unidades int not null,
	nombre_libro varchar(30) not null,
	entrega date not null,
	primary key (n_serie_libro, n_cuit_editorial),
	constraint FK_n_serie_libro_editoriales foreign key (n_serie_libro) references libros (n_serie_libro),
	constraint FK_n_cuit_editorial foreign key (n_cuit_editorial) references editorial (n_cuit_editorial));
