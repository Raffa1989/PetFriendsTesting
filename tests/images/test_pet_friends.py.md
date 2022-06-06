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
    status, result = pf.get_api_key(email, password)
    assert status == 200
    assert 'key' in result
```    

```python
def test_get_all_pets_with_valid_key(filter=''):
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.get_list_of_pets(auth_key, filter)
    assert status == 200
    assert len(result['pets']) > 0
```    

```python
def test_post_new_friends(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['name'] == name
```    

```python
def test_post_new_friends_2(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['animal_type'] == animal_type
```    

```python
def test_post_new_friends_3(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['age'] == age
```    

```python
def test_post_new_friends_4(name='laki', animal_type='cat', age='10', pet_photo='catfoto.jpg'):
    pet_photo = os.path.join(os.path.dirname(__file__), pet_photo)
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    status, result = pf.post_new_friends(auth_key, name, animal_type, age, pet_photo)
    assert status == 200
    assert result['pet_photo'] != C:\\Users\\Татьяна\\PycharmProjects\\PetFriendsTesting\\tests\\catfoto.jpg
```        

```python
def test_delete_pet():
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    pet_id = my_pets['pets'][62]['id']
    status, _ = pf.delete_pet(auth_key, pet_id)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    assert status == 200
    assert pet_id not in my_pets.values()
```    

```python
def test_put_self_pet_info(name='laki', animal_type='cat', age=10):
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    status, result = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    assert status == 200
    assert result['name'] == name
```    

```python
def test_put_self_pet_info_2(name='laki', animal_type='cat', age=10):
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    status, result = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    assert status == 200
    assert result['animal_type'] == animal_type
```
```python
def test_put_self_pet_info_3(name='laki', animal_type='cat', age=10):
    _, auth_key = pf.get_api_key(valid_email, valid_password)
    _, my_pets = pf.get_list_of_pets(auth_key, "my_pets")
    status, result = pf.put_pet_info(auth_key, my_pets['pets'][0]['id'], name, animal_type, age)
    assert status == 200
    assert result['age'] != age
```    
