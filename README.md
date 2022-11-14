# skillfactory-module-D.5.9

1. Создаем двух пользователей

from django.contrib.auth.models import User
user1 = User.objects.create_user('sasha', 'sasha@gmail.com', 'sashapassword')
user2 = User.objects.create_user('kostya', 'kostya@gmail.com', 'kostyapassword')


2. Создаем два объекта модели Author

from news.models import Authors
author1 = Authors.objects.create(id_user = user1, rating = 0)
author2 = Authors.objects.create(id_user = user2, rating = 0)

3. Добавили 5 категории в модель Category.

from news.models import Category
category1 = Category.objects.create(name_category = 'политика')
category2 = Category.objects.create(name_category = 'спорт')
category3 = Category.objects.create(name_category = 'образование')
category4 = Category.objects.create(name_category = 'наука')
category5 = Category.objects.create(name_category = 'футбол')

4. Добавили 2 статьи и 1 новость.

from news.models import Post
post1 = Post.objects.create(authors_id = author1, title = 'БАТЭ вышел в ЛГ', text_post = 'Самые первые сенсации БАТЭ пришлись на дебютный сезон в групповом турнире Лиги чемпионов.' )
post2 = Post.objects.create(authors_id = author2, type_post = Post.article, title = 'О животных, интересные факты', text_post = 'Хамелеоны могут двигать глазами в разных направлениях одновременно' )
post3 = Post.objects.create(authors_id = author2, type_post = Post.article, title = 'О рыбах, интересные факты', text_post = 'Плавать в вертикальном положении могут только морские коньки' )

5. Присвоили им категории (как минимум в одной статье/новости должно быть не меньше 2 категорий).

from news.models import PostCategory
post_cat1 = PostCategory.objects.create(id_post = post2, id_category = category3)
post_cat2 = PostCategory.objects.create(id_post = post2, id_category = category4)
post_cat3 = PostCategory.objects.create(id_post = post3, id_category = category3)
post_cat4 = PostCategory.objects.create(id_post = post3, id_category = category4)
post_cat5 = PostCategory.objects.create(id_post = post1, id_category = category2)
post_cat6 = PostCategory.objects.create(id_post = post1, id_category = category5)

6. Создали 4 комментария к разным объектам модели Post.

from news.models import Comment
comment1 = Comment.objects.create(id_post = post1, id_user = user1, text_com = 'Ура!!! Мы это сделали!')
comment2 = Comment.objects.create(id_post = post2, id_user = user2, text_com = 'Интересно, не знал!!!')
comment3 = Comment.objects.create(id_post = post3, id_user = user2, text_com = 'Про это давно известно!!!')
comment4 = Comment.objects.create(id_post = post3, id_user = user1, text_com = 'Автор, ты уверен в информации?')

7. Применяя функции like() и dislike() к статьям/новостям и комментариям, скорректировfkb рейтинги этих объектов.

post1.like()
post1.like()
post2.like()
post3.like()
post3.like()
post3.like()
post1.dislike()

comment1.like()
comment1.like()
comment2.like()
comment3.like()
comment4.like()
comment4.like()
comment4.dislike()

8. Обновили рейтинги пользователей
author1.update_rating()
author2.update_rating()

9. Вывели user_name и рейтинг лучшего (ответ в словаре answer)

author_top=Authors.objects.all().order_by('-rating').values('id_user', 'rating')[0]
user_name_top=User.objects.filter(id=author_top['id_user']).values('username')[0]
author_top.pop('id_user')
answer = {**user_name_top, **author_top}

10. Вывели username автора, дату добавления, рейтинг, заголовок и превью лучшей статьи, основываясь на лайках/дислайках к этой статье ((ответ в словаре answer))

post_top = Post.objects.all().order_by('-rating_post').values('id', 'time_in', 'authors_id', 'rating_post', 'title')[0]
author_top = Authors.objects.filter(id=post_top['authors_id']).values('id_user')[0]
user_name_top=User.objects.filter(id=author_top['id_user']).values('username')[0]
preview_post_top = Post.objects.get(id=post_top['id']).preview()
post_top['preview_post'] = preview_post_top
id_post_top = post_top['id']
post_top.pop('id')
post_top.pop('authors_id')
answer = {**user_name_top, **post_top}

11. Вывели все комментарии (дата, пользователь, рейтинг, текст) к этой статье

comments_post_top = Comment.objects.filter(id_post=id_post_top).values('time_in', 'id_user_id', 'rating_com', 'text_com')














