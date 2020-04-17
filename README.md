# HyppeDesafio

## Instruções Banco de dados
 - Execute para rodar o banco
 ```sh
 docker-compose -f stack.yml up
```

 - Execute o script para poder modelar o banco
 ```sh
CREATE TABLE IF NOT EXISTS "__EFMigrationsHistory" (
    "MigrationId" character varying(150) NOT NULL,
    "ProductVersion" character varying(32) NOT NULL,
    CONSTRAINT "PK___EFMigrationsHistory" PRIMARY KEY ("MigrationId")
);

CREATE TABLE "Events" (
    "Id" integer NOT NULL GENERATED BY DEFAULT AS IDENTITY,
    "Name" text NOT NULL,
    "Date" timestamp without time zone NOT NULL,
    CONSTRAINT "PK_Events" PRIMARY KEY ("Id")
);

CREATE TABLE "Users" (
    "Id" integer NOT NULL GENERATED BY DEFAULT AS IDENTITY,
    "Email" text NOT NULL,
    "Password" text NOT NULL,
    "EventId" integer NULL,
    CONSTRAINT "PK_Users" PRIMARY KEY ("Id"),
    CONSTRAINT "FK_Users_Events_EventId" FOREIGN KEY ("EventId") REFERENCES "Events" ("Id") ON DELETE RESTRICT
);

CREATE INDEX "IX_Users_EventId" ON "Users" ("EventId");

INSERT INTO "__EFMigrationsHistory" ("MigrationId", "ProductVersion")
VALUES ('20200416000612_InitialMigration', '3.1.3');

INSERT INTO "__EFMigrationsHistory" ("MigrationId", "ProductVersion")
VALUES ('20200416001129_Initial', '3.1.3');

ALTER TABLE "Users" DROP CONSTRAINT "FK_Users_Events_EventId";

DROP INDEX "IX_Users_EventId";

ALTER TABLE "Users" DROP COLUMN "EventId";

CREATE TABLE "EventUsers" (
    "UserId" integer NOT NULL,
    "EventId" integer NOT NULL,
    "Id" integer NOT NULL,
    "Status" boolean NOT NULL,
    CONSTRAINT "PK_EventUsers" PRIMARY KEY ("UserId", "EventId"),
    CONSTRAINT "FK_EventUsers_Events_EventId" FOREIGN KEY ("EventId") REFERENCES "Events" ("Id") ON DELETE CASCADE,
    CONSTRAINT "FK_EventUsers_Users_UserId" FOREIGN KEY ("UserId") REFERENCES "Users" ("Id") ON DELETE CASCADE
);

CREATE INDEX "IX_EventUsers_EventId" ON "EventUsers" ("EventId");

INSERT INTO "__EFMigrationsHistory" ("MigrationId", "ProductVersion")
VALUES ('20200416004209_Initial2', '3.1.3');

ALTER TABLE "EventUsers" DROP CONSTRAINT "PK_EventUsers";

ALTER TABLE "EventUsers" ALTER COLUMN "Id" TYPE integer;
ALTER TABLE "EventUsers" ALTER COLUMN "Id" SET NOT NULL;
ALTER TABLE "EventUsers" ALTER COLUMN "Id" ADD GENERATED BY DEFAULT AS IDENTITY;

ALTER TABLE "EventUsers" ADD CONSTRAINT "PK_EventUsers" PRIMARY KEY ("Id");

CREATE INDEX "IX_EventUsers_UserId" ON "EventUsers" ("UserId");

INSERT INTO "__EFMigrationsHistory" ("MigrationId", "ProductVersion")
VALUES ('20200416004745_Initial3', '3.1.3');

ALTER TABLE "Events" ADD "UserCreatorId" integer NULL;

CREATE INDEX "IX_Events_UserCreatorId" ON "Events" ("UserCreatorId");

ALTER TABLE "Events" ADD CONSTRAINT "FK_Events_Users_UserCreatorId" FOREIGN KEY ("UserCreatorId") REFERENCES "Users" ("Id") ON DELETE RESTRICT;

INSERT INTO "__EFMigrationsHistory" ("MigrationId", "ProductVersion")
VALUES ('20200416174035_Initial4', '3.1.3');
```

## Instruções para Executar a api
procure por bin>debug>netcoreapp3.1 e execute HyppeDesafio.exe
 

