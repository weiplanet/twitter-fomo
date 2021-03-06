# Migration `20200912191941-fix-relationship`

This migration has been generated by Tom Dohnal at 9/12/2020, 9:19:41 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "public"."TweetType" DROP CONSTRAINT "TweetType_tweetId_fkey"

ALTER TABLE "public"."TweetType" DROP COLUMN "tweetId"

CREATE TABLE "public"."_TweetToTweetType" (
"A" integer   NOT NULL ,
"B" integer   NOT NULL 
)

CREATE UNIQUE INDEX "_TweetToTweetType_AB_unique" ON "public"."_TweetToTweetType"("A", "B")

CREATE INDEX "_TweetToTweetType_B_index" ON "public"."_TweetToTweetType"("B")

ALTER TABLE "public"."_TweetToTweetType" ADD FOREIGN KEY ("A")REFERENCES "public"."Tweet"("id") ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE "public"."_TweetToTweetType" ADD FOREIGN KEY ("B")REFERENCES "public"."TweetType"("id") ON DELETE CASCADE ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200912191541-add-unique-to-tweet-type..20200912191941-fix-relationship
--- datamodel.dml
+++ datamodel.dml
@@ -2,9 +2,9 @@
 // learn more about it in the docs: https://pris.ly/d/prisma-schema
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url = "***"
 }
 generator client {
   provider        = "prisma-client-js"
@@ -39,10 +39,9 @@
   id        Int      @id @default(autoincrement())
   name      String   @unique
   createdAt DateTime @default(now())
   updatedAt DateTime @updatedAt
-  tweet     Tweet    @relation(fields: [tweetId], references: [id])
-  tweetId   Int
+  tweets    Tweet[]  @relation(references: [id])
 }
 model TweetHashtag {
   id        Int      @id @default(autoincrement())
@@ -145,9 +144,9 @@
   media                  TweetMedia[]
   urls                   TweetUrl[]
   userMentions           TweetUserMention[]
   symbols                TweetSymbol[]
-  tweetTypes             TweetType[]        @relation
+  tweetTypes             TweetType[]        @relation(references: [id])
   createdAt              DateTime           @default(now())
   updatedAt              DateTime           @updatedAt
   TweetUnwoundUrl        TweetUnwoundUrl[]
 }
```


