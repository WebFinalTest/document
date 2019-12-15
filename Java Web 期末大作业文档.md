[TOC]

# 一、数据库设计

## User表

| 意义         | 类型   | 备注        |
| ------------ | ------ | ----------- |
| 用户Id       | Long   | 唯一        |
| 用户昵称     | 字符串 |             |
| 用户邮箱     | 字符串 | 唯一        |
| 用户登录密码 | 字符串 | 不为空      |
| 用户积分     | Long   | 默认为0     |
| 管理员身份   | 布尔型 | 默认为false |
| QQ           | Long   | 可以为空    |
| 手机号码     | Long   | 可以为空    |
| 工作地点     | Long   | 可以为空    |
| 居住地       | Long   | 可以为空    |

**主键：**用户Id

## 帖子表

| 意义       | 数据类型 | 备注               |
| ---------- | -------- | ------------------ |
| 帖子Id     | Long     | 唯一               |
| 用户Id     | Long     |                    |
| 帖子标题   | 字符串   | 不为空             |
| 创建时间   | 时间     | 不为空             |
| 修改时间   | 时间     | 默认与创建时间一致 |
| 更新时间   | 时间     | 默认与创建时间一致 |
| 类型       | bool     |                    |
| 积分       | Long     |                    |
| 采纳评论Id | Long     |                    |
| 置顶       | bool     |                    |
| 加精       | bool     |                    |

**主键： **帖子Id

**外键： **用户Id **评论Id

## 评论表

| 意义     | 数据类型 | 备注   |
| -------- | -------- | ------ |
| 评论Id   | Long     | 唯一   |
| 用户Id   | Long     |        |
| 帖子Id   | Long     |        |
| 回复Id   | Long     |        |
| 层级Id   | Long     |        |
| 内容     | Long     | 不为空 |
| 创建时间 | 时间     | 不为空 |

**主键：**评论Id

**外键：**用户Id，帖子Id，回复Id,层级Id

同一个帖子的所有评论帖子Id相同

一级回复的回复Id默认为空

二级及上回复的回复Id为父评论Id，且层级Id为一级回复评论Id

## 点赞表

| 意义   | 数据类型 | 备注   |
| ------ | -------- | ------ |
| 帖子Id | Long     |        |
| 评论Id | Long     |        |
| 用户Id | Long     | 不为空 |

**外键：**帖子Id，评论Id，用户Id

## 收藏表

| 意义     | 数据类型 | 备注   |
| -------- | -------- | ------ |
| 帖子Id   | Long     |        |
| 用户Id   | Long     |        |

| 收藏时间 | 时间     | 不为空 |

**主键：**帖子Id，用户Id

**键：**收藏Id

# 二、接口文档

## 实体类

用户：```User```

帖子：```Post```

评论：```Comment```

点赞：```Like```

收藏：```Collect```

## 业务层

### 用户

#### 普通用户

1. 创建用户```User register(User user);```
2. 更新用户信息```bool update(User user);```
3. 用户登录```bool login(String email, String  pwd);```
4. 查找用户昵称是否被使用```bool isUsedByName(String name);```
5. 查找邮箱是否被使用```bool isUsedByEmail(String email);```

#### 管理员

### 帖子

1. 创建帖子```bool createPost(Post post);```
2. 加精帖子```bool qualityPost(Long postId);```
3. 置顶帖子```bool topPost(Long postId);```
4. 取消置顶```bool untopPost(Long postId);```
5. 删除帖子```bool deletePost(Post post);```
6. 修改帖子```bool updatePost(Post post);```
7. 查找所有帖子```List<Post> findPosts();```
8. 查找单个帖子```Post findPostByPostId(Long postId);```
9. 采纳评论```bool finishPost(Long postId, Long commentId);```

### 评论

1. 创建评论```bool createComment(Comment comment);```
2. 删除评论```bool deleteComment(Long commentId);```
3. 查找所有评论```List<Comment> findCommentsByPostId(Long postId);```
4. 用户查找评论```List<Comment> findCommentsByUserId(Long userId);```

### 点赞

1. 创建点赞```bool createLike(Good good);```
2. 查看点赞过的帖子```List<Like> findPostLikes(Long userId);```
3. 查看帖子点赞数```Long countPostLikes(Long postId);```
4. 查看帖子是否点赞过```bool isPostLike(Long postId);```
5. 查看点赞过的评论```List<Like> findCommentLikes(Long userId);```
6. 查看评论点赞数```Long countCommentLike(Long commentId);```
7. 查看评论是否点赞过```bool isCommentLike(Long commentId);```
8. 删除点赞```bool deleteLike(Long likeId);```

### 收藏

1. 创建收藏```bool createCollect(Collect collect)```
2. 查看收藏过的帖子```List<Collect> findPostCollects(Long userId)```
3. 查看帖子收藏数```Long countPostCollect(Long postId);```
4. 查看帖子是否收藏过```bool isPostCollect(Long postId);```
5. 查看收藏过的评论```List<Collect> findCommentCollects(Long userId);```
6. 查看评论收藏数```Long countCommentCollect(Long collectId);```
7. 查看评论是否收藏过```bool isCommentCollect(Long collectId);```
8. 删除收藏```bool deleteCollect(Long collectId);```
