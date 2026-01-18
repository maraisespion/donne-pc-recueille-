notice pour la mise an route - ouvrire windows powersell en tant que administrateur -ouvrire "god mod txt " dans block note sinon sa mache pas -copie coller le fichier(scripte) dans la console windows powersell pui entre a tendre 5 sec et un fichier HTML sais mi sur le bureaux ouvrer le et tata sais cadeaux 
ne pas prendre an compte "GOD MODE HACKER" sais juste un nom





# ==============================================
# GOD MODE HACKER - RAPPORT SYSTÈME DÉTAILLÉ & MATRIX STYLE
# ==============================================

$Desktop = [Environment]::GetFolderPath("Desktop")
$OutputFile = "$Desktop\GOD_MODE_HACKER_REPORT_MATRIX.html"

function ToHtmlTable($objects, $columns = $null) {
    if (-not $objects) { return "<p>Aucune donnée disponible.</p>" }
    if (-not $columns) {
        $columns = $objects[0].PSObject.Properties.Name
    }
    $html = "<table><thead><tr>"
    foreach ($col in $columns) {
        $html += "<th>$col</th>"
    }
    $html += "</tr></thead><tbody>"
    foreach ($obj in $objects) {
        $html += "<tr>"
        foreach ($col in $columns) {
            $val = $obj.$col
            $val = if ($val) { [System.Web.HttpUtility]::HtmlEncode($val.ToString()) } else { "" }
            $html += "<td>$val</td>"
        }
        $html += "</tr>"
    }
    $html += "</tbody></table>"
    return $html
}

function Section($title, $content) {
    return "<section class='section'><h2>$title</h2><div class='box'>$content</div></section>"
}

function Get-MemoryTypeName($type) {
    switch ($type) {
        20 {"DDR"}
        21 {"DDR2"}
        24 {"DDR3"}
        26 {"DDR4"}
        27 {"DDR5"}
        default {"Inconnu"}
    }
}

$HtmlHeader = @"
<!DOCTYPE html>
<html lang='fr'>
<head>
<meta charset='UTF-8'>
<title>GOD MODE HACKER — Rapport système avec fond Matrix</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap');

body {
    margin: 0; padding: 20px;
    font-family: 'Share Tech Mono', monospace;
    color: #00ff66;
    background-color: black;
    overflow-x: hidden;
    position: relative;
    min-height: 100vh;
}
h1 {
    text-align: center;
    text-shadow: 0 0 20px #00ff66;
    margin-bottom: 30px;
}
.section {
    margin-bottom: 40px;
    position: relative;
    z-index: 2;
}
.section h2 {
    border-left: 5px solid #00ff66;
    padding-left: 12px;
    margin-bottom: 10px;
}
.box {
    background-color: rgba(0, 17, 0, 0.85);
    border: 1px solid #00ff66;
    box-shadow: 0 0 15px #00ff66;
    padding: 15px;
    max-height: 400px;
    overflow-y: auto;
    white-space: normal;
}
table {
    border-collapse: collapse;
    width: 100%;
    font-size: 13px;
}
th, td {
    border: 1px solid #00ff66;
    padding: 6px 8px;
    text-align: left;
}
th {
    background-color: #003300;
}
tr:nth-child(even) {
    background-color: #002200;
}
.footer {
    text-align: center;
    margin-top: 50px;
    opacity: 0.5;
    font-size: 14px;
    position: relative;
    z-index: 2;
}

/* Scrollbar style */
::-webkit-scrollbar {
    width: 10px;
}
::-webkit-scrollbar-thumb {
    background: #00ff66;
    border-radius: 6px;
}

/* Canvas matrix background */
#matrix {
    position: fixed;
    top: 0; left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 1;
    pointer-events: none;
}
</style>
</head>
<body>
<canvas id='matrix'></canvas>
<p style='text-align:center; font-weight:bold; font-size:24px; color:#00ff66; text-shadow: 0 0 15px #00ff66; margin-bottom:10px; z-index:2; position:relative;'>
    GOD MOD CARACTÉRISTIQUE
</p>
<h1>☠ GOD MODE HACKER — RAPPORT SYSTÈME DÉTAILLÉ & MATRIX ☠</h1>
"@

$HtmlFooter = @"
<div class='footer'>Rapport généré le $(Get-Date)</div>

<script>
const canvas = document.getElementById('matrix');
const ctx = canvas.getContext('2d');

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const letters = 'abcdefghijklmnopqrstuvwxyz0123456789@#$%^&*+-=<>'.split('');
const fontSize = 16;
const columns = canvas.width / fontSize;

const drops = new Array(Math.floor(columns)).fill(1);

function draw() {
    ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    ctx.fillStyle = '#00ff66';
    ctx.font = fontSize + 'px monospace';

    for (let i = 0; i < drops.length; i++) {
        const text = letters[Math.floor(Math.random() * letters.length)];
        ctx.fillText(text, i * fontSize, drops[i] * fontSize);

        if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
            drops[i] = 0;
        }
        drops[i]++;
    }
}

setInterval(draw, 50);

window.addEventListener('resize', () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
});
</script>
</body>
</html>
"@

# === COLLECTE DES DONNÉES ===

# Infos système générales
$compInfo = Get-ComputerInfo

# BIOS
$bios = Get-CimInstance Win32_BIOS

# Carte mère avancée
$baseBoard = Get-CimInstance Win32_BaseBoard | Select-Object Manufacturer, Product, SerialNumber, Version, SKU, Tag

# CPU
$cpu = Get-CimInstance Win32_Processor

# RAM
$ramModules = Get-CimInstance Win32_PhysicalMemory
$ramTotalBytes = ($ramModules | Measure-Object Capacity -Sum).Sum
$ramTotalGB = [Math]::Round($ramTotalBytes / 1GB, 2)

# Disques physiques + SMART (nécessite admin)
$disks = Get-CimInstance Win32_DiskDrive
$physicalDisks = Get-PhysicalDisk -ErrorAction SilentlyContinue

# GPU
$gpus = Get-CimInstance Win32_VideoController

# Réseau
$netAdapters = Get-NetAdapter | Where-Object { $_.Status -eq "Up" }
$netIPConfig = Get-NetIPConfiguration | Select-Object InterfaceAlias, IPv4Address, IPv6Address, DNSServer, IPv4DefaultGateway, IPv6DefaultGateway

# Utilisateurs et groupes locaux
$users = Get-LocalUser | Select-Object Name, Enabled, LastLogon, Description
$groups = Get-LocalGroup | Select-Object Name, Description

# Services
$services = Get-Service | Sort-Object Status, DisplayName | Select-Object Status, Name, DisplayName

# Processus top 100 CPU
$processes = Get-Process | Sort-Object CPU -Descending | Select-Object -First 100 | Select-Object Id, ProcessName, CPU, @{Name="Mémoire(MB)";Expression={[Math]::Round($_.WorkingSet/1MB,2)}}

# Mises à jour Windows
$updates = Get-HotFix | Select-Object HotFixID, Description, InstalledOn

# Batterie (si portable)
$battery = Get-CimInstance Win32_Battery

# === CONSTRUCTION DU CONTENU HTML ===

$html = $HtmlHeader

# --- SYSTÈME & BIOS ---
$sysDetails = @"
<p><b>Nom OS :</b> $($compInfo.OsName)</p>
<p><b>Version OS :</b> $($compInfo.OsVersion) (Build $($compInfo.OsBuildNumber))</p>
<p><b>Architecture :</b> $($compInfo.OsArchitecture)</p>
<p><b>Nom ordinateur :</b> $($compInfo.CsName)</p>
<p><b>Utilisateur connecté :</b> $($compInfo.CsUserName)</p>
<p><b>Version BIOS :</b> $($bios.SMBIOSBIOSVersion)</p>
<p><b>Date BIOS :</b> $($bios.ReleaseDate)</p>
<p><b>Fabricant BIOS :</b> $($bios.Manufacturer)</p>
<p><b>Numéro de série BIOS :</b> $($bios.SerialNumber)</p>
"@
$html += Section "Système & BIOS" $sysDetails

# --- CARTE MÈRE ---
$baseBoardDetails = ""
$baseBoard | ForEach-Object {
    $baseBoardDetails += "<p><b>Fabricant :</b> $($_.Manufacturer)</p>"
    $baseBoardDetails += "<p><b>Produit :</b> $($_.Product)</p>"
    $baseBoardDetails += "<p><b>Numéro de série :</b> $($_.SerialNumber)</p>"
    $baseBoardDetails += "<p><b>Version :</b> $($_.Version)</p>"
    $baseBoardDetails += "<p><b>SKU :</b> $($_.SKU)</p>"
    $baseBoardDetails += "<p><b>Tag :</b> $($_.Tag)</p>"
}
$html += Section "Carte mère" $baseBoardDetails

# --- CPU ---
$cpuDetails = ""
foreach ($cpuItem in $cpu) {
    $cpuDetails += "<p><b>Nom :</b> $($cpuItem.Name)</p>"
    $cpuDetails += "<p><b>Cœurs physiques :</b> $($cpuItem.NumberOfCores)</p>"
    $cpuDetails += "<p><b>Cœurs logiques :</b> $($cpuItem.NumberOfLogicalProcessors)</p>"
    $cpuDetails += "<p><b>Fréquence max :</b> $($cpuItem.MaxClockSpeed) MHz</p>"
    $cpuDetails += "<p><b>Architecture :</b> $($cpuItem.Architecture)</p>"
    $cpuDetails += "<hr>"
}
$html += Section "Processeur (CPU)" $cpuDetails

# --- RAM DÉTAILLÉE ---
$ramDetailed = foreach ($mod in $ramModules) {
    [PSCustomObject]@{
        BankLabel    = $mod.BankLabel
        CapacityGB   = [Math]::Round($mod.Capacity / 1GB, 2)
        SpeedMHz     = $mod.Speed
        Manufacturer = $mod.Manufacturer
        MemoryType   = Get-MemoryTypeName $mod.MemoryType
        SerialNumber = $mod.SerialNumber
        PartNumber   = $mod.PartNumber
        ConfiguredVoltage = if ($mod.ConfiguredVoltage) { "$($mod.ConfiguredVoltage) mV" } else { "N/A" }
        DataWidth   = $mod.DataWidth
        TotalWidth  = $mod.TotalWidth
        DeviceLocator = $mod.DeviceLocator
    }
}
$ramSummary = "<p><b>RAM totale :</b> $ramTotalGB GB</p>"
$ramTable = ToHtmlTable $ramDetailed
$html += Section "Mémoire RAM (détails par module)" "$ramSummary$ramTable"

# --- DISQUES PHYSIQUES + ÉTAT SMART (si possible) ---
$diskTable = $disks | Select-Object Model, SerialNumber, @{Name="Taille (GB)";Expression={[Math]::Round($_.Size / 1GB,2)}}, MediaType, InterfaceType
$html += Section "Disques physiques" (ToHtmlTable $diskTable)

if ($physicalDisks) {
    $smartTable = $physicalDisks | Select-Object FriendlyName, SerialNumber, MediaType, OperationalStatus, HealthStatus, Size, BusType
    $html += Section "Disques physiques (État SMART)" (ToHtmlTable $smartTable)
}

# --- GPU ---
$gpuTable = $gpus | Select-Object Name, DriverVersion, VideoProcessor, @{Name="Mémoire (MB)";Expression={[Math]::Round($_.AdapterRAM / 1MB,2)}}
$html += Section "Carte graphique (GPU)" (ToHtmlTable $gpuTable)

# --- RÉSEAU ---
$netAdapterTable = $netAdapters | Select-Object Name, InterfaceDescription, MacAddress, Status, LinkSpeed, DriverVersion
$html += Section "Adaptateurs réseau actifs" (ToHtmlTable $netAdapterTable)

# IP configuration détaillée (plus lisible)
$ipDetails = ""
foreach ($ipconf in $netIPConfig) {
    $ipDetails += "<p><b>Interface :</b> $($ipconf.InterfaceAlias)</p>"
    $ipDetails += "<p><b>IPv4 :</b> $($ipconf.IPv4Address.IPAddress -join ', ')</p>"
    $ipDetails += "<p><b>IPv6 :</b> $($ipconf.IPv6Address.IPAddress -join ', ')</p>"
    $ipDetails += "<p><b>Passerelle IPv4 :</b> $($ipconf.IPv4DefaultGateway.NextHop)</p>"
    $ipDetails += "<p><b>Passerelle IPv6 :</b> $($ipconf.IPv6DefaultGateway.NextHop)</p>"
    $ipDetails += "<p><b>Serveurs DNS :</b> $($ipconf.DNSServer.ServerAddresses -join ', ')</p>"
    $ipDetails += "<hr>"
}
$html += Section "Configuration IP détaillée" $ipDetails

# --- UTILISATEURS ---
$userDetails = ToHtmlTable $users @("Name","Enabled","LastLogon","Description")
$html += Section "Utilisateurs locaux" $userDetails

$groupDetails = ToHtmlTable $groups @("Name","Description")
$html += Section "Groupes locaux" $groupDetails

# --- SERVICES ---
$serviceDetails = ToHtmlTable $services @("Status","Name","DisplayName")
$html += Section "Services Windows" $serviceDetails

# --- PROCESSUS ---
$processDetails = ToHtmlTable $processes @("Id","ProcessName","CPU","Mémoire(MB)")
$html += Section "Processus actifs (top 100 CPU)" $processDetails

# --- MISES À JOUR ---
$updateDetails = ToHtmlTable $updates @("HotFixID","Description","InstalledOn")
$html += Section "Mises à jour Windows" $updateDetails

# --- BATTERIE ---
if ($battery) {
    $batteryDetails = ""
    foreach ($bat in $battery) {
        $batteryDetails += "<p><b>Nom :</b> $($bat.Name)</p>"
        $batteryDetails += "<p><b>État :</b> $($bat.BatteryStatus)</p>"
        $batteryDetails += "<p><b>Charge restante :</b> $($bat.EstimatedChargeRemaining)%</p>"
        $batteryDetails += "<hr>"
    }
    $html += Section "Batterie (portable)" $batteryDetails
}

$html += $HtmlFooter

# Écriture du fichier
$html | Out-File -Encoding UTF8 $OutputFile

Write-Host "✔ Rapport GOD MODE MATRIX généré sur le Bureau" -ForegroundColor Green
Write-Host $OutputFile -ForegroundColor Green
