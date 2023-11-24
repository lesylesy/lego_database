# lego_database
BEGIN;

CREATE TABLE IF NOT EXISTS public.colors
(
    id integer NOT NULL,
    name character varying(255) COLLATE pg_catalog."default",
    rgb character varying(255) COLLATE pg_catalog."default",
    is_trans character varying(255) COLLATE pg_catalog."default",
    CONSTRAINT colors_pkey PRIMARY KEY (id)
);

CREATE TABLE IF NOT EXISTS public.inventories
(
    id integer NOT NULL,
    version integer,
    set_num character varying(255) COLLATE pg_catalog."default",
    CONSTRAINT inventories_pkey PRIMARY KEY (id)
);

CREATE TABLE IF NOT EXISTS public.inventory_parts
(
    inventory_id integer,
    part_num character varying(255) COLLATE pg_catalog."default",
    color_id integer,
    quantity integer,
    is_spare character varying(255) COLLATE pg_catalog."default"
);

CREATE TABLE IF NOT EXISTS public.inventory_sets
(
    inventory_id integer,
    set_num character varying(255) COLLATE pg_catalog."default",
    quantity integer
);

CREATE TABLE IF NOT EXISTS public.part_categories
(
    id integer NOT NULL,
    name character varying(255) COLLATE pg_catalog."default",
    CONSTRAINT part_categories_pkey PRIMARY KEY (id)
);

CREATE TABLE IF NOT EXISTS public.parts
(
    part_num character varying(255) COLLATE pg_catalog."default" NOT NULL,
    name character varying(255) COLLATE pg_catalog."default",
    part_cat_id integer,
    CONSTRAINT parts_pkey PRIMARY KEY (part_num)
);

CREATE TABLE IF NOT EXISTS public.sets
(
    set_num character varying(255) COLLATE pg_catalog."default" NOT NULL,
    name character varying(255) COLLATE pg_catalog."default",
    year integer,
    theme_id integer,
    num_parts integer,
    CONSTRAINT sets_pkey PRIMARY KEY (set_num)
);

CREATE TABLE IF NOT EXISTS public.themes
(
    id integer NOT NULL,
    name character varying COLLATE pg_catalog."default" NOT NULL,
    parent_id integer NOT NULL,
    CONSTRAINT themes_pkey PRIMARY KEY (id)
);

ALTER TABLE IF EXISTS public.inventories
    ADD CONSTRAINT set_num FOREIGN KEY (set_num)
    REFERENCES public.sets (set_num) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION;


ALTER TABLE IF EXISTS public.inventory_parts
    ADD CONSTRAINT color_id FOREIGN KEY (color_id)
    REFERENCES public.colors (id) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION;


ALTER TABLE IF EXISTS public.inventory_parts
    ADD CONSTRAINT inventory_id FOREIGN KEY (inventory_id)
    REFERENCES public.inventories (id) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION;


ALTER TABLE IF EXISTS public.inventory_sets
    ADD CONSTRAINT inventory_id FOREIGN KEY (inventory_id)
    REFERENCES public.inventories (id) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION;


ALTER TABLE IF EXISTS public.inventory_sets
    ADD CONSTRAINT set_num FOREIGN KEY (set_num)
    REFERENCES public.sets (set_num) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION;


ALTER TABLE IF EXISTS public.parts
    ADD CONSTRAINT part_cat_id FOREIGN KEY (part_cat_id)
    REFERENCES public.part_categories (id) MATCH SIMPLE
    ON UPDATE NO ACTION
    ON DELETE NO ACTION;

END;
