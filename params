// ==UserScript==
// @name        Bennynator - params.cgi
// @namespace   legit.club
// @description Modification Serveurs Atelier
// @include     https://interne.ovh.ca/atelier/astreinte/params.cgi*
// @version     1
// @grant       GM_setClipboard
// ==/UserScript==

// General notes
// addEventListener sont wrapper dans une function sinon l'event trigger quand la page load

// Ajout du bouton "popout" 
var test = {};
// J'ai mis ces 4 lignes en dehors du "if" parceque je fais référence à "servernum" ailleurs dans le code!
var findth = document.getElementsByTagName("th"); // On se passe sur le th du haut (Paramètres N° xxxxxx)
var servernumraw = findth[0].textContent; // notre th est le premier du document donc 0
var servernum = servernumraw.substring(servernumraw.length -6, servernumraw.length);
var serverurl = "https://interne.ovh.ca/atelier/astreinte/control.cgi?nummachine=" + servernum;
if (test = window.parent.frameElement) { // On check si le parent (normallement control.cgi) est lui aussi dans un frame (lorsque loader depuis /atelier/astreinte)
var btn_popout = document.createElement("input");
btn_popout.type = "button";
btn_popout.value = "//";
btn_popout.title = "Reload sans le menu astreinte."
btn_popout.addEventListener("click", function() { window.open(serverurl, "_top","", "false") }, false); // Reload dans la même fenêtre
findth[0].appendChild(btn_popout);
}

// On trouve le IP du serveur
var txtip = document.getElementsByName("ip"), ips = "", ipinfo="";
ips = txtip[0].value.trim(); // String du IP.
txtip[0].addEventListener("click", function() { GM_setClipboard(ips); }, false);

// On trouve le nom du serveur (hostname)
var txtname = document.getElementsByName("nom"), host = "";
host = txtname[0].value.trim();
txtname[0].addEventListener("click", function() { GM_setClipboard(host); }, false);

// On trouve la baie du serveur 
var txtbaie = document.getElementsByName("baie"), baie = "";
baie = txtbaie[0].value.trim();
txtbaie[0].addEventListener("click", function() { GM_setClipboard(baie); }, false);

// On trouve le IP et la MAC de l'IPMI (all hail IPMI)
var aipmi = document.getElementsByTagName("a"), ipmi = "", macipmi = "";
for (i=0;i<aipmi.length;i++) {
  if (aipmi[i].textContent.substring(0,4) == "IPMI") {
    var findipmi = aipmi[i].textContent;
    macipmi = findipmi.substring(findipmi.length - 17, findipmi.length); // Les dernier caractères du lien forme la MAC, simple.
    // On détermine si l'IPMI est tagged ou dédié pour extraire correctement le IPMI.
    if (findipmi.substring(4,5) == "t") {
      // On se positionne après "tagged" et avant la mac     
      ipmi = findipmi.substring(11, findipmi.length - 18 );
    } else {
      // On se positionne après "IPMI" et avant la mac
      ipmi = findipmi.substring(5, findipmi.length - 18);
    }
    // Ajoute un bouton qui envoie le IP de l'IPMI dans le clipboard.
    var cpipmi = document.createElement("input");
    cpipmi.type="button";
    cpipmi.value="copy IP";
    cpipmi.title="Copy IP to paste elsewhere";
    cpipmi.addEventListener("click", function() { GM_setClipboard(ipmi); }, false);
    aipmi[i].parentNode.appendChild(cpipmi);
    // Double click dans le TD copy l'ipmi
    aipmi[i].parentNode.addEventListener("dblclick", function() { GM_setClipboard(ipmi); }, false);
    aipmi[i].parentNode.title="Double click me to copy the IPMI IP";
    break;
  }
}

// On trouve la MAC du serveur et le port et le IP de la switch.

// infobulle est le lien (<a>) dans la case avec l'info switch, on s'en sert pour trouver l'info dans le "td" parent.
var aswitch = document.getElementsByClassName("infobulle"), port = "", ipswitch = "", mac = "";
if (aswitch[0]) { // le code est exécuté seulement si une switch d'identifié sur le serveur vu qu'il cherche le link "?".
var findinfo = aswitch[0].parentNode.textContent; 
ipswitch = findinfo.substring(0, findinfo.indexOf(" ")-1);
port = findinfo.substring(findinfo.indexOf("("), findinfo.indexOf(")")+1);
mac = findinfo.substring(findinfo.length - 25, findinfo.length - 8).toUpperCase();
// On ajoute les boutons pour copier le IP ou le port.
var btn_ipswitch = document.createElement("input"), btn_port = document.createElement("input");
// Bouton IP switch
btn_ipswitch.type="button";
btn_ipswitch.id="copyipswitch";
btn_ipswitch.value="IP";
btn_ipswitch.title="Copy IP to clipboard";
btn_ipswitch.addEventListener("click", function() { GM_setClipboard(ipswitch); }, false);
// Bouton port switch
btn_port.type="button";
btn_port.id="copyswitchport";
btn_port.value="port";
btn_port.title="Copy switch port to clipboard";
btn_port.addEventListener("click", function() { GM_setClipboard(port); }, false);
// On les ajoute au document
aswitch[0].parentNode.appendChild(btn_ipswitch);
aswitch[0].parentNode.appendChild(btn_port);
}


// On cré les bouton IP/FW (il est ajouté au document dans la boucle plus bas)
// Bouton IP
var btn_ip = document.createElement("input");
btn_ip.type="button";
btn_ip.id="ip";
btn_ip.value="IP";
btn_ip.title="Ouvrir https://interne.ovh.ca/ip/#/infos?ip=";
btn_ip.addEventListener("click", openip);
// Bouton FW
var btn_fw = document.createElement("input");
btn_fw.type = "button";
btn_fw.id = "fw";
btn_fw.value = "FW";
btn_fw.title = "FireWall check";
btn_fw.addEventListener("click", fwcheck);

ipinfo = "https://interne.ovh.ca/ip/#/infos?ip=" + ips; // URL pour l'interface IP
// On trouve la case (td) contenant "eth0" et on ajoute les boutons à côté
var tdeth = document.body.getElementsByTagName("td");
for (i=0; i < tdeth.length; i++) {   
  // substring (0,4) ajouté dans les cas ou il y a plusieurs interface ex : eth0 [ + 5 extra ] on extrait donc les 4 premiers char soit eth0
  if (tdeth[i].textContent.substring(0,4).trim() == "eth0") {
    tdeth[i].appendChild(btn_ip);
    tdeth[i].appendChild(btn_fw);
  }
}

function openip() {  // utiliser par btn_ip
   window.open(ipinfo, "_blank");   
}

function fwcheck() {  // utiliser par btn_fw
  var fwurl = "https://interne.ovh.ca/monitoring/v3/firewall.cgi?host=" + host 
  if(confirm("Check Firewall ?")){window.open(fwurl,"Firewall","toolbar=0,menubar=0,location=0,scrollbars=1,width=850,height=550")};
}

// Bouton Info générale (pour donner l'info sur la ML/N2)
// Inclus hostname, IP, ip de l'ipmi, ip du switch, port sur le switch.
var btn_infos = document.createElement("input"), infos = "";
btn_infos.type="button";
btn_infos.id="copyallinfo";
btn_infos.value="copy server info";
btn_infos.title="Copy: Name, IP, IP IPMI, MAC, Switch IP + Port";
btn_infos.addEventListener("click", copyall);
txtname[0].parentNode.appendChild(btn_infos);

function copyall() {  
  // On détermine si le serveur a IPMI et on copy l'info appropriée
  if (ipmi != "") {
    GM_setClipboard("Nom: "+host+"\n"+"IP: "+ips+"\n"+"MAC: "+mac+"\n"+"IPMI: "+ipmi+"\n"+"MAC IPMI: "+macipmi+"\n"+"Switch: "+ipswitch+"\n"+"Port: "+port);
  } else {
    GM_setClipboard("Nom: "+host+"\n"+"IP: "+ips+"\n"+"MAC: "+mac+"\n"+"Switch: "+ipswitch+"\n"+"Port: "+port);
  }
  
}

// Ajout bouton qui ouvre ip:81
// On se positionne par le dropdownmenu de netboot.
var trouve_kernel = document.getElementsByName("boot_kernel"), btn_81 = document.createElement("input"), url81 = "http://" + ips + ":81";
btn_81.type="button";
btn_81.id="port81";
btn_81.value=":81";
btn_81.title="Ouvrir la page " +ips+":81";
btn_81.addEventListener("click", function() { window.open(url81, "_BLANK") }, false);
trouve_kernel[0].parentNode.appendChild(btn_81);


// On change le titre de la fenêtre pour BAIE + 3 dernier digit + dernier byte IP
var lastnum = servernum.substring(servernum.length - 3, servernum.length);
var tbaie = baie.substring(baie.length - 4, baie.length);
var lastbyte = ips.substring(ips.length - 3, ips.length);
if (lastbyte.indexOf(".") > -1) {   // 3 char si fini par 2 digit, 4 char si fini par 3 digit
  } else {
    lastbyte = ips.substring(ips.length - 4, ips.length);
}

top.document.title = tbaie + "/" + lastnum +  "/" + lastbyte;



// test
var testframe = parent.document.getElementsByTagName("frame");
var parentdoc = parent.document;

