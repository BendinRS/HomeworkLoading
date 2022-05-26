# Домашняя работа "Загрузка Linux"

 <details>
  <summary>Попасть в систему без пароля несколькими способами </summary>
   

+ Открываем GUI Hyper-V и запускаем заранее подготовленную ВМ
+ При выборе ядра нажимаем **е** 


#### Способ 1. init=/bin/sh

+ В конце строки начинающейся с linux16 добавляем init=/bin/sh и нажимаем сtrl-x для загрузки в систему 
+ Мы попали в систему. Но рутовая файловая
система при этом монтируется в режиме Read-Only. Если мы хотим перемонтировать ее в
режим Read-Write можно воспользоваться командой:
```out1
mount -o remount,rw /
```
![test](https://i.ibb.co/Lt0P67L/loading.png)

#### Способ 2. rd.break

+ В конце строки начинающейся с linux16 добавляем rd.break и нажимаем сtrl-x для загрузки в систему
+ Попадаем в emergency mode. Наша корневая файловая система смонтирована (опять же в режиме Read-Only, но мы не в ней.Далее пример как попасть в нее и поменять пароль администратора:
![test](https://i.ibb.co/3pG9qn6/1.png)

+ После чего можно перезагружаться и заходить в систему с новым паролем. Полезно когда вы потеряли или вообще не имели пароль администратора.

#### Способ 3. rw init=/sysroot/bin/sh

+ В строке начинающейся с linux16 заменяем ro на rw init=/sysroot/bin/sh и нажимаем сtrl-x для загрузки в систему
+ В целом то же самое что и в прошлом примере, но файловая система сразу смонтирована в режим Read-Write
+ В прошлых примерах тоже можно заменить ro на rw

 </details>

<details>
  <summary>Установить систему с LVM, после чего переименовать VG </summary>

  + Посмотрим текущее состояние системы, узнаем имя volume group и переименуем его:
  ![test](https://i.ibb.co/dbTzNNH/loading3.png)

  + Далее правим /etc/fstab, /etc/default/grub, /boot/grub2/grub.cfg. Везде заменяем старое название на новое.
  + Пересоздаем initrd image, чтобы он знал новое название Volume Group
  ```out2
  mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)
  ```
  ![test](https://i.ibb.co/Jz5FffF/loading4.png)

  + Перезагрузка

  + У-успех
  ![test](https://i.ibb.co/K90w679/5.png)
</details>


<details>
  <summary>Добавить модуль в initrd </summary>

+ Скрипты модулей хранятся в каталоге /usr/lib/dracut/modules.d/. Для того чтобы добавить свой модуль - создаем там папку с именем 01test
+ Помещаем в папку два скрипта [module-setup.sh](module-setup.sh) и [test.sh](test.sh)
+ Пересобираем образ initrd
```out4
mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)
```

+ В /boot/grub2/grub.cfg убираем опции rghb и quiet

+ Ребут

+ У-успех
![test](https://i.ibb.co/9vQ72Sn/loading7.png)

</details>