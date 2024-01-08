# sql_hw4
1. Найдите общее количество лайков, которые получили пользователи женского пола.

SELECT COUNT(*) -- количество лайков
FROM likes l
JOIN media m ON l.media_id = m.id
JOIN profiles p ON p.user_id = m.user_id
WHERE p.gender = 'f';
-------
2. Найдите количество лайков, которые поставили пользователи женского пола и мужского пола.
Выведите название пола (преобразовав значение атрибута f в женский, а значение ‘m` - мужской) и
количество, поставленных лайков, применив к количеству лайков сортировку по убыванию.

SELECT 
CASE p.gender
WHEN 'f' THEN 'женский'
WHEN 'm' THEN 'мужской'
END AS gender, 
COUNT(*) AS cnt
FROM likes l 
JOIN profiles p on l.user_id = p.user_id
GROUP BY p.gender 
ORDER BY cnt DESC;

------
3. Выведите идентификаторы пользователей, которые не отправляли ни одного сообщения.

SELECT 
u.id
FROM users u 
LEFT JOIN messages m on u.id = m.from_user_id
WHERE m.id IS NULL;

------
4. Найдите количество друзей у каждого пользователя. Выведите для каждого пользователя идентификатор 
пользователя, фамилию, имя и количество друзей. Сортировка выводимых записей в порядке возрастания 
идентификатора пользователя.

SELECT 
u.id, 
u.firstname, 
u.lastname, 
COUNT(fr.status) AS cnt 
FROM users u
LEFT JOIN friend_requests fr ON (u.id = fr.initiator_user_id or u.id = fr.target_user_id) AND fr.status = 'approved'
GROUP BY u.id
ORDER BY u.id;
