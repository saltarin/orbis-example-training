Ciclo de comandos para resolver el problema de archivo pesado:

1- Cuenta los objetos de GIT:
git count-objects -v		

2- Limpia la BD guardando todos los objetos en un archivo empaquetador.
git gc							

3- Visualiza los objetos pesados ordenados por la 3ra columna que indica el peso:
git verify-pack -v .git/objects/pack/pack-e121f70e46f7ec3df6d405dfb4895762265c6af7.idx

Se visualiza el HASH del objeto más pesado:
cfc3322ec593c88d0e4d68c312de3583ca741041 blob   153942735 153904153 471677	

4- Listar los archivos del HASH:
git rev-list --objects --all | grep 

Se visualiza el archivo:
cfc3322ec593c88d0e4d68c312de3583ca741041 sc.16.tar.gz							
5- Se borra la cache de GIT considerando el archivo más pesado:
git filter-branch --index-filter 'git rm --cached --ignore-unmatch sc.16.tar.gz'							

6- Se pushea los cambios al remoto:
git push origin master


-- intrucciones: 
git gc 
git count-objects -v
git verify-pack -v .git/objects/pack/pack-afebcfefee638013d8de4fd9460481f6d88ef2f8.idx | sort -k 3 -n | tail -3

git rev-list --objects --all | grep cfc3322ec593c88d0e4d68c312de3583ca741041
git log --pretty=oneline --branches -- sc.16.tar.gz

3bd289d96f8784ba7a3fd793364df80ebaaec1f3 seventh commit
f3a6dc74d8fd32278c93a5e9d4bbaec4d43c04c5 fifth commit

(primera forma)
git filter-branch --index-filter \
   'git rm --cached --ignore-unmatch sc.16.tar.gz' -- f3a6dc^..

(segunda forma)
git filter-branch --index-filter \
   'git rm --cached --ignore-unmatch sc.16.tar.gz' -- --all

rm -Rf .git/refs/original
rm -Rf .git/logs/

git gc
git count-objects -v