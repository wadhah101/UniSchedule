# Migration `20201111130809-lo`

This migration has been generated by Wadhah mahrouk at 11/11/2020, 2:08:09 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "SchoolTime" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "dayId" INTEGER NOT NULL,
    "start" DATETIME NOT NULL,
    "end" DATETIME NOT NULL,
    "group" INTEGER,
    "week" TEXT,
    "schoolSessionId" INTEGER,

    FOREIGN KEY ("dayId") REFERENCES "WeekDay"("id") ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY ("schoolSessionId") REFERENCES "SchoolSession"("id") ON DELETE SET NULL ON UPDATE CASCADE
)

CREATE TABLE "SchoolSession" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "subjectId" INTEGER NOT NULL,
    "professorId" INTEGER NOT NULL,
    "sessionTypeId" INTEGER NOT NULL,

    FOREIGN KEY ("subjectId") REFERENCES "Subject"("id") ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY ("professorId") REFERENCES "Professor"("id") ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY ("sessionTypeId") REFERENCES "SessionType"("id") ON DELETE CASCADE ON UPDATE CASCADE
)
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20201111000934-ss..20201111130809-lo
--- datamodel.dml
+++ datamodel.dml
@@ -1,10 +1,7 @@
-// This is your Prisma schema file,
-// learn more about it in the docs: https://pris.ly/d/prisma-schema
-
 datasource db {
   provider = "sqlite"
-  url = "***"
+  url = "***"
 }
 generator client {
   provider = "prisma-client-js"
@@ -44,24 +41,14 @@
   filiereWithLevelId Int
   SchoolSession      SchoolSession[]
 }
-model SchoolPeriod {
-  id              Int          @id @default(autoincrement())
-  hour            Int
-  minute          Int
-  schoolTimeStart SchoolTime[] @relation("start")
-  schoolTimeEnd   SchoolTime[] @relation("end")
-}
-
 model SchoolTime {
   id              Int            @id @default(autoincrement())
   day             WeekDay        @relation(fields: [dayId], references: [id])
   dayId           Int
-  start           SchoolPeriod   @relation(fields: [startId], references: [id], name: "start")
-  startId         Int
-  end             SchoolPeriod   @relation(fields: [endId], references: [id], name: "end")
-  endId           Int
+  start           DateTime
+  end             DateTime
   group           Int?
   week            String?
   SchoolSession   SchoolSession? @relation(fields: [schoolSessionId], references: [id])
   schoolSessionId Int?
```

