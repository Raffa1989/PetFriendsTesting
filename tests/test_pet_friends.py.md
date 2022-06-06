```python
import os
```
```python
from api import PetFriends
from setting import valid_email, valid_password
```

```python
pf = PetFriends()
```

```python
def test_get_api_key_for_valid_user(email=valid_email, password=valid_password):
    """ Проверяем что запрос api ключа возвращает статус 200 и в результате содержится слово key"""
    status, result = pf.get_api_key(email, password)
    assert status == 200
    assert 'key' in result
```    

```python
def test_get_all_pets_with_valid_key(filter=''):
    """ Проверяем что запрос всех питомцев возвращает не пустой список.
        Для этого сначала получаем api ключ и сохраняем в переменную auth_key. Далее используя этого ключ
        запрашиваем список всех питомцев и проверяем что список не пустой.
        Доступное значение параметра filter - 'my_pets' либо '' """
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.get_list_of_pets(auth_key, filter)
    assert status == 200
    assert len(result['pets']) > 0
```    

```python
def test_post_new_friends(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    """Проверяем что можно добавить питомца с корректными данными (по кличке)"""
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['name'] == name
```    

```python
def test_post_new_friends_2(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    """Проверяем что можно добавить питомца с корректными данными (по виду)"""
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['animal_type'] == animal_type
```    

```python
def test_post_new_friends_3(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    """Проверяем что можно добавить питомца с корректными данными (по возрасту)"""
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['age'] == age
```    

```python
def test_post_new_friends_4(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    """Проверяем что можно добавить питомца с корректными данными (по фото)"""
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['pet_photo'] != pet_photo
```    

```python
def test_delete_pet():
    """Проверяем возможность удаления питомца"""
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    pet_id = my_pets['pets'][62]['id']
    status, _ = pf.delete_pet(auth_key, pet_id)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    assert status == 200
    assert pet_id not in my_pets.values()
```    

```python
def test_delete_pet_2(name='laki', animal_type='cat', age=10):
    """Проверяем возможность удаления клички питомца"""
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    pet_id = my_pets['pets'][0]['id']
    status, _ = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    assert status == 200
    assert name not in pet_id
```    

```python
def test_delete_pet_3(name='laki', animal_type='cat', age=10):
    """Проверяем возможность удаления вида питомца"""
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    pet_id = my_pets['pets'][68]['id']
    status, _ = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    assert status == 200
    assert animal_type not in pet_id
```    

```python
def test_delete_pet_4(name='laki', animal_type='cat', age= '10'):
    """Проверяем возможность удаления возраста питомца"""
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    pet_id = my_pets['pets'][68]['id']
    status, _ = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    assert status == 200
    assert age not in pet_id
```    

```python
def test_put_self_pet_info(name='laki', animal_type='cat', age=10):
    """Проверяем возможность обновления информации о кличке питомца"""
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    status, result = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    assert status == 200
    assert result['name'] == name
```    

```python
def test_put_self_pet_info_2(name='laki', animal_type='cat', age=10):
    """Проверяем возможность обновления информации о виде питомца"""
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    status, result = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    assert status == 200
    assert result['animal_type'] == animal_type
```    

```python
def test_put_self_pet_info_3(name='laki', animal_type='cat', age=10):
    """Проверяем возможность обновления информации о возрасте питомца"""
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    status, result = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    assert status == 200
    assert result['age'] != age
```
