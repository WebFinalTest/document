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

**主键：** 帖子Id

**外键：** 用户Id评论Id

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

**主键：** 评论Id

**外键：** 用户Id，帖子Id，回复Id,层级Id

同一个帖子的所有评论帖子Id相同

一级回复的回复Id默认为空

二级及上回复的回复Id为父评论Id，且层级Id为一级回复评论Id

## 点赞表

| 意义   | 数据类型 | 备注   |
| ------ | -------- | ------ |
| 帖子Id | Long     |        |
| 评论Id | Long     |        |
| 用户Id | Long     | 不为空 |

**外键：** 帖子Id，评论Id，用户Id

## 收藏表

| 意义     | 数据类型 | 备注   |
| -------- | -------- | ------ |
| 帖子Id   | Long     |        |
| 用户Id   | Long     |        |
| 收藏时间 | 时间     | 不为空 |
| 收藏夹Id | Long     | 不为空 |

**主键：** 帖子Id，用户Id，收藏夹Id

**外键：** 帖子Id，用户Id，收藏夹Id

## 收藏夹表

| 意义     | 数据类型 | 备注   |
| -------- | -------- | ------ |
| 收藏夹Id   | Long     |        |
| 用户Id   | Long     |        |
| 收藏夹名字 | Long     | 不为空 |

**主键：** 收藏夹Id

**外键：** 用户Id

**id长度：** userId: 9，postId:10，commentId:12，favaritesid:11


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
6. 返回所有用户```public List<User> findAll();```
7. 按照id查询user```public User findByUserId(Long userId);```

#### 管理员（详见下方对帖子的操作）
1. 加精帖子
2. 置顶帖子
3. 取消置顶
3. 删除帖子

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
10. 取消加精```public void unQualityPost(Long postId);```  
11. 取消置顶```public void unTopPost(Long postId);```  
12. 采纳评论```public void adoptComment(Long postId, Long commentId);```  
13. 修改标题```public void updatePostTitle(Long postId, String postTitle);```  
14. 修改帖子内容```public void updatePostContent(Long postId, String postContent);```  
15. 查找所有帖子```public List<Post> findPosts();```  
16. 分页查找帖子```public List<Post> findPostsByPage(Long page);```  
17. 查找某个用户的所有帖子```public List<Post> findPostsByUserId(Long userId);```  
18. 查找某个用户某页的所有帖子```public List<Post> findPostsByPageUserId(Long userId, Long page);```  
19. 查找某个帖子```public Post findPostByPostId(Long postId);```  
20. 计算所有帖子页数```public Long countPostsPage();```  
21. 计算某个用户所有帖子页数```public Long countPostsPageByUserId(Long userId);```  

### 评论

1. 创建评论```bool createComment(Comment comment);```
2. 删除评论```bool deleteComment(Long commentId);```
3. 查找所有评论```List<Comment> findCommentsByPostId(Long postId);```
4. 用户查找评论```List<Comment> findCommentsByUserId(Long userId);```
5. 返回一个帖子的某页的评论```public List<Comment> findByPostId(Long postId,Long page);```
6. 返回一个层级某页的评论```public List<Comment> findByFloorId(Long floorId,Long page);```
7. 返回一个层级评论页数```public Long countCommentPagesByFloorId(Long floorId);```
8. 返回一个用户某页的评论```public List<Comment> findByUserId(Long userId,Long page);```
9. 返回一个用户评论页数```public Long countCommentPagesByUserId(Long userId);```

### 点赞

1. 创建点赞```bool createLike(Good good);```
2. 查看点赞过的帖子```List<Like> findPostLikes(Long userId);```
3. 查看帖子点赞数```Long countPostLikes(Long postId);```
4. 查看帖子是否点赞过```bool isPostLike(Long postId);```
5. 查看点赞过的评论```List<Like> findCommentLikes(Long userId);```
6. 查看评论点赞数```Long countCommentLike(Long commentId);```
7. 查看评论是否点赞过```bool isCommentLike(Long commentId);```
8. 删除点赞```bool deleteLike(Long likeId);```
9. 返回一个帖子评论页数```public Long countCommentPagesByPostId(Long postId);```


### 收藏

1. 创建收藏```bool createCollect(Collect collect)```
2. 创建收藏夹```bool createFavorites(Favorites favorites)```
3. 查看我的收藏夹```List<Favorites> findFavoritesById(Long userId)```
4. 查看收藏过的帖子```List<Collect> findPostCollects(Long FavoritesId)```
5. 删除收藏```bool deleteCollect(Long collectId);```
6. 删除收藏夹（只限空收藏夹，提前判断）```bool deleteFavorites(Long favoritesId);```
7. 修改收藏夹```bool updateFavorites(Long favoritesId);```
8. 修改收藏```bool updateCollect(Long collectId);```


##表现层

###用户
1. 修改用户信息```public Map doUpdateUserInfo(HttpSession session,User user)```
2. 修改用户密码``` public Map<String,String> updatePassword(HttpSession session,String originalPassword,String newPassword)```
3. 检查用户名是否被使用```public Map checkUserName(String userName,HttpSession session)```
4. 登录```public Map doLogin(User user, HttpSession session)```
5. 注册```public Map doRegister```

###帖子
1. 根据页码查看帖子```public List<Post> viewByPage(@PathVariable("page") Long page)```
2. 创建帖子```public Map<String,String> create(Post post, HttpSession session)```
3. 删除帖子```public Map deleteByPostId(Long postId)```
4. 加精帖子```public Map qualityPost(Long postId)```
5. 取消加精帖子```public Map unQualityPost(Long postId)```
6. 置顶帖子```public Map topPost(Long postId)```
7. 取消置顶帖子```public Map unTopPost(Long postId)```
8. 更新帖子```public Map updatePost (Post post)```
9. 查看我创建的帖子```public String showMyPosts(HttpSession session,Model model)```
10. 采纳评论（给予积分）```public Map adoptPost(Long postId,Long commentId)```

###点赞
1. 给帖子点赞```public Map likePost(HttpSession session,Long postId) ```
2. 给评论点赞```public Map likeComment(HttpSession session, Long commentId)```

###收藏
1. 创建新的收藏夹```public Map createFavorites(HttpSession session,String favoritesName)```
2. 查看我的收藏夹```public List<Favorites> viewFavorites(HttpSession session)```
3. 修改收藏夹```public Map renameFavorites(Favorites favorites)```
4. 删除收藏夹```public Map deleteFavorites(Long favoritesId)```
5. 把收藏添加到收藏夹```public Map createCollect(HttpSession session ,Long postId)```
6. 改变收藏位置```public Map replaceCollect(Collect collect)```
7. 查看一个收藏夹的全部收藏```public String viewCollect(Long favoritesId, Model model)```
8. 删除一个收藏```public Map deleteCollect(HttpSession session,Long postId)```

###评论
1. 创建评论```public Map createComment (HttpSession session,Comment comment)```
2. 查看层级评论```public String goFloorComments(@PathVariable("floorId") Long floorId,Model model)```
