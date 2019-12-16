# 数据库语句
* 先执行：
```
select version(),
@@sql_mode;SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```
* 建立user表：
```
CREATE TABLE `bbsforum`.`user`  (
  `userId` bigint(20) NOT NULL,
  `userName` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `password` varchar(30) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,
  `email` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `userType` tinyint(3) NOT NULL,
  `points` bigint(20) NOT NULL DEFAULT 0,
  `qq` bigint(20) NULL DEFAULT NULL,
  `phone` bigint(20) NULL DEFAULT NULL,
  `workplace` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `habitation` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`userId`) USING BTREE,
  UNIQUE INDEX `username`(`userName`) USING BTREE,
  UNIQUE INDEX `email`(`email`) USING BTREE,
  INDEX `userName_2`(`userName`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 11 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```
* 建立post表：
```
CREATE TABLE `bbsforum`.`post`  (
    `postId` bigint(20) NOT NULL,
  `userId` bigint(20) NOT NULL,
  `postTitle` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `postContent` varchar(10000) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `createDate` datetime(0) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updateDate` datetime(0) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `renewDate` datetime(0) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `top` tinyint(3) NOT NULL DEFAULT 0,
  `quality` tinyint(3) NOT NULL DEFAULT 0,
  `postType` tinyint(3) NOT NULL,
  `postPoints` bigint(20) NULL DEFAULT NULL,
  `adoptCommentId` bigint(20) NULL DEFAULT NULL,
  PRIMARY KEY (`postId`) USING BTREE,
  INDEX `post_ibfk_1`(`userId`) USING BTREE,
  INDEX `commentId`(`adoptCommentId`) USING BTREE,
  CONSTRAINT `post_ibfk_1` FOREIGN KEY (`userId`) REFERENCES `bbsforum`.`user` (`userId`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```
* 建立comment表：
```
CREATE TABLE `bbsforum`.`comment`  (
  `commentId` bigint(20) NOT NULL,
  `postId` bigint(20) NOT NULL,
  `userId` bigint(20) NOT NULL,
  `commentContent` varchar(1000) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `replyId` bigint(20) NULL DEFAULT NULL,
  `floorId` bigint(20) NULL DEFAULT NULL,
  `creatDate` datetime(0) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`commentId`) USING BTREE,
  INDEX `postId`(`postId`) USING BTREE,
  INDEX `comment_ibfk_1`(`replyId`) USING BTREE,
  INDEX `comment_ibfk_2`(`floorId`) USING BTREE,
  INDEX `userId`(`userId`) USING BTREE,
  CONSTRAINT `comment_ibfk_1` FOREIGN KEY (`replyId`) REFERENCES `bbsforum`.`comment` (`commentId`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `comment_ibfk_2` FOREIGN KEY (`floorId`) REFERENCES `bbsforum`.`comment` (`commentId`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `comment_ibfk_3` FOREIGN KEY (`postId`) REFERENCES `bbsforum`.`post` (`postId`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `comment_ibfk_4` FOREIGN KEY (`userId`) REFERENCES `bbsforum`.`user` (`userId`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```
* 插入外键：
```
ALTER TABLE `bbsforum`.`post` 
ADD CONSTRAINT `post_ibfk_2` FOREIGN KEY (`adoptCommentId`) REFERENCES `bbsforum`.`comment` (`commentId`) ON DELETE RESTRICT ON UPDATE CASCADE;
```
* 建立like表：
```
CREATE TABLE `bbsforum`.`like`  (
  `postId` bigint(20) NULL DEFAULT NULL,
  `commentId` bigint(20) NULL DEFAULT NULL,
  `userId` bigint(20) NOT NULL,
  UNIQUE INDEX `commentId`(`commentId`, `userId`) USING BTREE,
  INDEX `userId`(`userId`) USING BTREE,
  UNIQUE INDEX `xx`(`postId`, `userId`) USING BTREE,
  INDEX `postId`(`postId`) USING BTREE,
  CONSTRAINT `like_ibfk_1` FOREIGN KEY (`postId`) REFERENCES `bbsforum`.`post` (`postId`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `like_ibfk_2` FOREIGN KEY (`commentId`) REFERENCES `bbsforum`.`comment` (`commentId`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `like_ibfk_3` FOREIGN KEY (`userId`) REFERENCES `bbsforum`.`user` (`userId`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```
* 建立favorites表：
```
CREATE TABLE `bbsforum`.`favorites`  (
  `favoritesId` bigint(20) NOT NULL,
  `userId` bigint(20) NOT NULL,
  `favoritesName` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  PRIMARY KEY (`favoritesId`) USING BTREE,
  UNIQUE INDEX `userId`(`userId`, `favoritesName`) USING BTREE,
  CONSTRAINT `favorites_ibfk_1` FOREIGN KEY (`userId`) REFERENCES `bbsforum`.`user` (`userId`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```
* 建立collect表：
```
CREATE TABLE `bbsforum`.`collect`  (
  `postId` bigint(20) NOT NULL,
  `userId` bigint(20) NOT NULL,
  `collectDate` datetime(0) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `favoritesId` bigint(20) NOT NULL,
  PRIMARY KEY (`postId`, `userId`) USING BTREE,
  INDEX `userId`(`userId`) USING BTREE,
  INDEX `favoritesId`(`favoritesId`) USING BTREE,
  CONSTRAINT `collect_ibfk_1` FOREIGN KEY (`postId`) REFERENCES `bbsforum`.`post` (`postId`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `collect_ibfk_2` FOREIGN KEY (`userId`) REFERENCES `bbsforum`.`user` (`userId`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `collect_ibfk_3` FOREIGN KEY (`favoritesId`) REFERENCES `bbsforum`.`favorites` (`favoritesId`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;
```
