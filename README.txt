#вводить когда не лень
$ sudo apt update
$ sudo apt-get update


#Для начала надо поставить git, вот комманда для терминала:
$ sudo apt install git

#Затем установите AFL++ (просто вводите все комманды подряд всё получится):
$ git clone https://github.com/AFLplusplus/AFLplusplus
$ sudo apt-get update
$ sudo apt-get install -y build-essential python3-dev automake cmake git flex bison libglib2.0-dev libpixman-1-dev python3-setuptools cargo libgtk-3-dev
$ sudo apt-get install -y lld-14 llvm-14 llvm-14-dev clang-14 || sudo apt-get install -y lld llvm llvm-dev clang
$ sudo apt-get install -y gcc-$(gcc --version|head -n1|sed 's/\..*//'|sed 's/.* //')-plugin-dev libstdc++-$(gcc --version|head -n1|sed 's/\..*//'|sed 's/.* //')-dev
$ sudo apt-get install -y ninja-build
$ cd AFLplusplus
$ make distrib
$ sudo make install
#конец


#После этого напишите свою программу и скомпилируйте его как хотите, я в vscode делал
#Протестируйте работу программы в терминале:
$ ./[название скомпиленного файла] <<<[входные данные]

#В папке с cpp и скомпиленым файлом создайте файл CMakeLists.txt с следующим содержанием:
#
#
cmake_minimum_required (VERSION 3.8)
set (CMAKE_CXX_STANDARD 14)
project ([название скомпиленного файла])
add_executable ([название скомпиленного файла] [файл cpp])
#
#

#Там же создайте еще одну папку для билда через afl (у меня это afl_build)
#В ней создайте папку с входными данными, пару примеров фазеру будет достаточно:
$ mkdir afl_build
$ cd $_
$ mkdir [файл с фходными данными]
$ cd $_
$ echo "[первые входные данные]" >01.txt
$ echo "[вторые входные данные]" >02.txt

#В терминале, находясь в папке билда через afl введите следующие комманды:
$ CC=/[путь до AFL++]/afl-gcc CXX=/[путь до AFL++]/afl-g++ cmake ..
$ make

#Запустите фазер!
$ /[путь до AFL++]/afl-fuzz -i [файл с фходными данными] -o ../[файл для выходных данных] ./[название скомпиленного файла]

#Если высветилась ошибка с крешами:
$ sudo -s #зайти с рута на чутка
#ИЛИ
$ sudo passwd root #пароль поменять на руте
$ su root #тут с паролем надо зайти
#Из рута написать:
$ echo core >/proc/sys/kernel/core_pattern #исправление
$ exit #выйти с рута

#Точно пригодится:
Гитхаб с AFL++: https://github.com/AFLplusplus/AFLplusplus/blob/stable/docs/INSTALL.md
Гитхаб с примером теста: https://github.com/wolframroesler/afl-demo/blob/master/README.md

