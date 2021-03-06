---
title: Konfigurera PostgreSQL på en virtuell Linux-dator
description: Lär dig hur du installerar och konfigurerar PostgreSQL på en virtuell Linux-dator i Azure
author: cynthn
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: d86e42dcc16d108cc82c9d245c7919145cef365f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759343"
---
# <a name="install-and-configure-postgresql-on-azure"></a>Installera och konfigurera PostgreSQL på Azure
PostgreSQL är en avancerad databas med öppen källkod som liknar Oracle och DB2. Den innehåller företags klara funktioner som full syra efterlevnad, tillförlitlig transaktions bearbetning och samtidighet med flera versioner. Det stöder också standarder som ANSI SQL och SQL/with (inklusive externa data omslutningar för Oracle, MySQL, MongoDB och många andra). Den är mycket utöknings bar med stöd för över 12 procedur språk, GIN-och register index, spatiala data stöd och flera NoSQL-liknande funktioner för JSON-eller nyckel värdebaserade program.

I den här artikeln får du lära dig hur du installerar och konfigurerar PostgreSQL på en virtuell Azure-dator som kör Linux.


## <a name="install-postgresql"></a>Installera PostgreSQL
> [!NOTE]
> Du måste redan ha en virtuell Azure-dator som kör Linux för att kunna slutföra den här självstudien. Information om hur du skapar och konfigurerar en virtuell Linux-dator innan du fortsätter finns i [själv studie kursen om virtuella Azure Linux-datorer](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

I det här fallet använder du Port 1999 som PostgreSQL-port.  

Anslut till den virtuella Linux-dator som du skapade via SparaTillFil. Om det här är första gången du använder en virtuell Azure Linux-dator läser du så här [använder du SSH med Linux på Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för att lära dig hur du använder SparaTillFil för att ansluta till en virtuell Linux-dator.

1. Kör följande kommando för att växla till roten (admin):
   
        # sudo su -
2. Vissa distributioner har beroenden som du måste installera innan du installerar PostgreSQL. Sök efter din distribution i listan och kör lämpligt kommando:
   
   * Red Hat Base Linux:
     
           # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
   * Debian Base Linux:
     
            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  
   * SUSE Linux:
     
           # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  
3. Ladda ned PostgreSQL till rot katalogen och packa sedan upp paketet:
   
        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/
   
        # tar jxvf  postgresql-9.3.5.tar.bz2
   
    Ovanstående är ett exempel. Du kan hitta den mer detaljerade nedladdnings adressen i [indexet för/pub/source/](https://ftp.postgresql.org/pub/source/).
4. Starta versionen genom att köra följande kommandon:
   
        # cd postgresql-9.3.5
   
        # ./configure --prefix=/opt/postgresql-9.3.5
5. Om du vill skapa allt som kan skapas, inklusive dokumentationen (HTML-och man-sidor) och ytterligare moduler (`contrib`), kör du följande kommando i stället:
   
        # gmake install-world
   
    Du bör få följande bekräftelse meddelande:
   
        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>Konfigurera PostgreSQL
1. Valfritt Skapa en symbolisk länk för att förkorta PostgreSQL-referensen så att den inte innehåller versions numret:
   
        # ln -s /opt/postgresql-9.3.5 /opt/pgsql
2. Skapa en katalog för databasen:
   
        # mkdir -p /opt/pgsql_data
3. Skapa en icke-rot användare och ändra användarens profil. Växla sedan till den nya användaren (kallas *postgres* i vårt exempel):
   
        # useradd postgres
   
        # chown -R postgres.postgres /opt/pgsql_data
   
        # su - postgres
   
   > [!NOTE]
   > Av säkerhets skäl använder PostgreSQL en icke-rot användare för att initiera, starta eller stänga av databasen.
   > 
   > 
4. Redigera *bash_profile* -filen genom att ange följande kommandon. Dessa rader kommer att läggas till i slutet av *bash_profiles* filen:
   
        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF
5. Kör *bash_profile* -filen:
   
        $ source .bash_profile
6. Verifiera installationen med hjälp av följande kommando:
   
        $ which psql
   
    Om installationen lyckas visas följande svar:
   
        /opt/pgsql/bin/psql
7. Du kan också kontrol lera PostgreSQL-versionen:
   
        $ psql -V

8. Initiera databasen:
   
        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W
   
    Du bör få följande utdata:

![image](./media/postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>Konfigurera PostgreSQL
<!--    [postgres@ test ~]$ exit -->

Kör följande kommandon:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Ändra två variabler i/etc/init.d/postgresql-filen. Prefixet anges till installations Sök vägen för PostgreSQL: **/opt/pgsql**. PGDATA har angetts till data lagrings Sök vägen för PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/postgresql-install/no2.png)

Ändra filen för att göra den körbar:

    # chmod +x /etc/init.d/postgresql

Starta PostgreSQL:

    # /etc/init.d/postgresql start

Kontrol lera om slut punkten för PostgreSQL är på:

    # netstat -tunlp|grep 1999

Du bör se följande utdata:

![image](./media/postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Anslut till postgres-databasen
Växla till postgres-användaren en gång igen:

    # su - postgres

Skapa en postgres-databas:

    $ createdb events

Anslut till händelse databasen som du nyss skapade:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Skapa och ta bort en postgres-tabell
Nu när du har anslutit till databasen kan du skapa tabeller i den.

Skapa till exempel en ny exempel postgres-tabell med hjälp av följande kommando:

    CREATE TABLE potluck (name VARCHAR(20),    food VARCHAR(30),    confirmed CHAR(1), signup_date DATE);

Nu har du skapat en tabell med fyra kolumner med följande kolumn namn och begränsningar:

1. Kolumnen "namn" har begränsats av kommandot VARCHAR till var 20 tecken lång.
2. I kolumnen "mat" anges det mat objekt som varje person ska ta. VARCHAR begränsar den här texten till 30 tecken.
3. Kolumnen "bekräftad" anger om personen har RSVP till knytkalas. De acceptabla värdena är "Y" och "N".
4. Kolumnen "datum" visar när de registrerade sig för händelsen. Postgres kräver att datum skrivs som åååå-mm-dd.

Du bör se följande om tabellen har skapats:

![image](./media/postgresql-install/no4.png)

Du kan också kontrol lera tabell strukturen med hjälp av följande kommando:

![image](./media/postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Lägga till data i en tabell
Börja med att infoga information i en rad:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Du bör se dessa utdata:

![image](./media/postgresql-install/no6.png)

Du kan också lägga till ett par fler personer i tabellen. Här följer några alternativ, eller så kan du skapa egna:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Visa tabeller
Använd följande kommando för att visa en tabell:

    select * from potluck;

Utdata ser ut så här:

![image](./media/postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Ta bort data i en tabell
Använd följande kommando för att ta bort data i en tabell:

    delete from potluck where name=’John’;

Detta tar bort all information på "John"-raden. Utdata ser ut så här:

![image](./media/postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Uppdatera data i en tabell
Använd följande kommando för att uppdatera data i en tabell. För den här sandbrun har bekräftat att de deltar, så vi kommer att ändra RSVP från "N" till "Y":

     UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


## <a name="get-more-information-about-postgresql"></a>Få mer information om PostgreSQL
Nu när du har slutfört installationen av PostgreSQL i en virtuell Azure Linux-dator kan du använda den i Azure. Mer information om PostgreSQL finns på postgresql- [webbplatsen](https://www.postgresql.org/).

