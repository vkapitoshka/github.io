---
layout: post
title: Соответствие дисков VMware и дисков в ОС Windows
description: "Использование различных тегов в поста но через jekyll"
modified: 2020-02-27
tags: [VMware]
---
> На момент написание данной статьи используется VMware 6.5

> По роду своей деятельности мне достаточно часто приходится выполнять работы по увеличение дисков на виртуальных машинах VMware. В своей среде, мы в основном используем Windows ОС. В целом, эта процедура не сложная:
* Находим виртуальную машину в консоли VMware
* Выбираем диск, который нужно увеличить в консоли VMware
* Увеличиваем диск

> На этом работа можно сказать закончена и остается только увеличить диск в ОС виртуальной машины. Данную процедуру я описывать здесь не буду, так как она проста и не вызывает сложностей.

> Теперь рассмотрим такой вариан, у виртуальной машины 1 **SCSI** контролер и два диска одинакового размера. Обычно ОС Windows диски нумеруются последовательно, т.е. диск 1 виртуальной машины будет соответствовать диск 0 в ОС Windows и так далее, но к сожалению это не всегда так, и лучше, убедиться что мы добавляем место именно тому диску на уровне Виртуализации, которому нужно.

На уровне виртуализации при подключении диска к одному и тому же **SCSI** контролеру каждому диск выдается порядковый номер. Таким образом каждый диск имеет свой идентификатор он состоит из
номера SCSI контролера,нумирация идет с 0, и номера под которым подключен диск. По умолчанию <u>первый диск</u> имеет адрес **0:0**, <u>второй</u> **0:1** и т.д. 

![vm_Disk]({{ sute.url }}/assets/images/vm_Disk.JPG)
{: .image-right}
Максимальнок количество дисков для одного SCSI контролера равно 16.

**Но как нам определить какой диск какой в ОС Windows?**

В этом нам поможет встроенная утилита **DISKPART**:

1. Открываем коносль CMD с повышением привилегий.
2. Выполняем команду **DISKPART** - в консоли вы перейдете в командную оболочку diskpart
3. Выполняем команду *list disk*, которая отобразит нам все диски подключенные к виртуальной машине

    ![List_Disks]({{ sute.url }}/assets/images/list_d.JPG)
    {: .image-right}

4. Выбираем один из дисков, для которого нам требуется определить его номер. Это делается командой *selest disk (0,1,2,3 и т.д.)*

    ![Select_Disks]({{ sute.url }}/assets/images/select_d.JPG)
    {: .image-right}

5. После того как нужный диск выбран, выполняем команду *detail disk*, чтобы посмотреть детали диска
6. В эти деталях нужно обратить внимание для пункт **Target** - это и есть вторая цифра в адресе диска в формате **0:0**, **0:1**...**0:15**

    ![Detail_Disks]({{ sute.url }}/assets/images/detail_d.JPG)
    {: .image-center}

7. Остается только переправерить соответсвие номеров в консоли **VMware** и в консоли **DISKPART** и выполнить стандартную процедуру увеличения диска описанную выше