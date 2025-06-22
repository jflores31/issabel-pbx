# Opción 2: Creación con PowerShell CLI

### a. Crear 6 Unidades Organizativas (OUs)

```PowerShell
# Comando para crear una Unidad Organizativa (OU) en Active Directory

New-ADOrganizationalUnit -Name "Empresa" -Path "DC=idat,DC=pe" -ProtectedFromAccidentalDeletion $true

# Metodo (1)
# Definir la lista de OUs a crear
$OUs = @(
    "Administracion", 
    "Ventas", 
    "Soporte", 
    "TI", 
    "RecursosHumanos",
    "Finanzas"
)

# Crear cada OU dentro de "OU=Empresa,DC=idat,DC=pe"

foreach ($OU in $OUs) {
    New-ADOrganizationalUnit -Name $OU `
        -Path "OU=Empresa,DC=idat,DC=pe" `
        -ProtectedFromAccidentalDeletion $true
}

# Metodo (2)
# Versión alternativa (con verificación de existencia)

foreach ($OU in $OUs) {
    if (-not (Get-ADOrganizationalUnit -Filter "Name -eq '$OU'")) {
        New-ADOrganizationalUnit -Name $OU -Path "OU=Empresa,DC=idat,DC=pe" -ProtectedFromAccidentalDeletion $true
    } else {
        Write-Warning "La OU '$OU' ya existe."
    }
}
```
### b. Crear 20 Usuarios en las OUs

```PowerShell
# Importar módulo necesario
Import-Module ActiveDirectory

# Definir lista de usuarios (sin tildes y con nombres normalizados)
$usuarios = @(
    @{Nombre="Carlos"; Apellido="Lopez"; Usuario="clopez"; OU="Ventas"},
    @{Nombre="Maria"; Apellido="Perez"; Usuario="mperez"; OU="Ventas"},
    @{Nombre="Juan"; Apellido="Torres"; Usuario="jtorres"; OU="Administracion"},
    @{Nombre="Ana"; Apellido="Rios"; Usuario="arios"; OU="Administracion"},
    @{Nombre="Pedro"; Apellido="Gomez"; Usuario="pgomez"; OU="Soporte"},
    @{Nombre="Laura"; Apellido="Diaz"; Usuario="ldiaz"; OU="Soporte"},
    @{Nombre="Miguel"; Apellido="Castro"; Usuario="mcastro"; OU="TI"},
    @{Nombre="Sofia"; Apellido="Herrera"; Usuario="sherrera"; OU="TI"},
    @{Nombre="Daniel"; Apellido="Ruiz"; Usuario="druiz"; OU="RecursosHumanos"},
    @{Nombre="Elena"; Apellido="Flores"; Usuario="eflores"; OU="Finanzas"}
)

# Contadores para estadísticas
$creados = 0
$existentes = 0
$errores = 0

foreach ($u in $usuarios) {
    $pathOU = "OU=$($u.OU),OU=Empresa,DC=idat,DC=pe"
    
    # Verificar si el usuario ya existe
    if (-not (Get-ADUser -Filter "SamAccountName -eq '$($u.Usuario)'" -ErrorAction SilentlyContinue)) {
        try {
            # Crear nuevo usuario
            New-ADUser -Name "$($u.Nombre) $($u.Apellido)" `
                -GivenName $u.Nombre `
                -Surname $u.Apellido `
                -DisplayName "$($u.Nombre) $($u.Apellido)" `
                -UserPrincipalName "$($u.Usuario)@idat.pe" `
                -SamAccountName $u.Usuario `
                -Path $pathOU `
                -AccountPassword (ConvertTo-SecureString "Password123!" -AsPlainText -Force) `
                -ChangePasswordAtLogon $false `
                -Enabled $true `
                -ErrorAction Stop
            
            Write-Host "[CREADO] Usuario $($u.Usuario) creado en $pathOU" -ForegroundColor Green
            $creados++
            
            # Opcional: Agregar a grupos según departamento
            # Add-ADGroupMember -Identity "GRUPO_$($u.OU)" -Members $u.Usuario
            
        } catch {
            Write-Host "[ERROR] Fallo al crear $($u.Usuario): $_" -ForegroundColor Red
            $errores++
        }
    } else {
        Write-Host "[EXISTE] El usuario $($u.Usuario) ya existe" -ForegroundColor Yellow
        $existentes++
    }
}
```
## c. Crear 6 Grupos y Asignar Usuarios

```PowerShell
# Importar módulo necesario
Import-Module ActiveDirectory

## -------------------------------
## 1. CREACIÓN DE GRUPOS ORGANIZACIONALES
## -------------------------------

# Definir los grupos con su OU correspondiente
$grupos = @(
    @{Nombre="Ventas_Managers"; OU="Ventas"},
    @{Nombre="Administracion_Directores"; OU="Administracion"},
    @{Nombre="Soporte_Tecnico"; OU="Soporte"},  # Eliminé el acento técnico
    @{Nombre="TI_Admins"; OU="TI"},
    @{Nombre="RecursosHumanos_Admins"; OU="RecursosHumanos"},
    @{Nombre="Finanzas_Directores"; OU="Finanzas"}
)

Write-Host "`nCreando grupos de seguridad..." -ForegroundColor Cyan

# Crear los grupos si no existen
foreach ($grupo in $grupos) {
    $grupoPath = "OU=$($grupo.OU),OU=Empresa,DC=idat,DC=pe"
    
    if (-not (Get-ADGroup -Filter "Name -eq '$($grupo.Nombre)'" -ErrorAction SilentlyContinue)) {
        try {
            New-ADGroup -Name $grupo.Nombre `
                -GroupScope Global `
                -GroupCategory Security `
                -Path $grupoPath `
                -Description "Grupo de $($grupo.Nombre) para el departamento $($grupo.OU)" `
                -ErrorAction Stop
            
            Write-Host "[CREADO] Grupo $($grupo.Nombre) en $grupoPath" -ForegroundColor Green
        } catch {
            Write-Host "[ERROR] No se pudo crear el grupo $($grupo.Nombre): $_" -ForegroundColor Red
        }
    } else {
        Write-Host "[EXISTE] El grupo $($grupo.Nombre) ya existe" -ForegroundColor Yellow
    }
}

## -------------------------------
## 2. ASIGNACIÓN DE USUARIOS A GRUPOS
## -------------------------------

Write-Host "`nAsignando usuarios a grupos..." -ForegroundColor Cyan

# Mapeo de usuarios a grupos (nombres sin tildes)
$gruposUsuarios = @{
    "Ventas_Managers" = @("clopez", "mperez", "lmartinez", "jgarcia")
    "Administracion_Directores" = @("jtorres", "arios", "rmendoza", "ilopez")
    "Soporte_Tecnico" = @("pgomez", "ldiaz", "ajimenez", "rmoreno")  # Coherencia con nombre de grupo
    "TI_Admins" = @("mcastro", "sherrera", "fruiz", "psanchez")
    "RecursosHumanos_Admins" = @("druiz", "dfernandez")
    "Finanzas_Directores" = @("eflores", "svega")
}

# Contadores para estadísticas
$asignacionesExitosas = 0
$usuariosNoEncontrados = 0
$erroresAsignacion = 0

# Versión corregida del bloque de asignación de usuarios
foreach ($grupo in $gruposUsuarios.Keys) {
    # Verificar si el grupo existe primero
    if (-not (Get-ADGroup -Filter "Name -eq '$grupo'" -ErrorAction SilentlyContinue)) {
        Write-Host "[ERROR] El grupo $grupo no existe, no se pueden asignar usuarios" -ForegroundColor Red
        continue
    }
    
    $usuarios = $gruposUsuarios[$grupo]
    foreach ($usuario in $usuarios) {
        try {
            if (Get-ADUser -Filter "SamAccountName -eq '$usuario'" -ErrorAction Stop) {
                Add-ADGroupMember -Identity $grupo -Members $usuario -ErrorAction Stop
                Write-Host "[ASIGNADO] $usuario agregado a $grupo" -ForegroundColor Green
                $asignacionesExitosas++
            } else {
                Write-Host "[NO ENCONTRADO] Usuario $usuario no existe en AD" -ForegroundColor Yellow
                $usuariosNoEncontrados++
            }
        } catch {
        $errorMsg = if ($_.Exception -match "already a member") {
            "[ADVERTENCIA] $usuario ya pertenece a $grupo"
        } else {
            "[ERROR] Asignación fallida: $($_.Exception.Message)"
            $erroresAsignacion++
        }
        
        Write-Host $errorMsg -ForegroundColor $(if ($errorMsg -match "ADVERTENCIA") { "Yellow" } else { "Red" })
       }
    }
}

## -------------------------------
## 3. RESUMEN FINAL
## -------------------------------

Write-Host "`nResumen de ejecución:" -ForegroundColor Cyan
Write-Host "Grupos creados: $($grupos.Count)" -ForegroundColor Blue
Write-Host "Asignaciones exitosas: $asignacionesExitosas" -ForegroundColor Green
Write-Host "Usuarios no encontrados: $usuariosNoEncontrados" -ForegroundColor Yellow
Write-Host "Errores en asignaciones: $erroresAsignacion" -ForegroundColor Red
```

# Verificación de la Implementación en Active Directory

## Comandos de Verificación Completa

### 1. Verificación de Unidades Organizativas (OUs)
```PowerShell
# Listar todas las OUs con su estructura completa
Get-ADOrganizationalUnit -Filter * -Properties CanonicalName | 
Select-Object Name, DistinguishedName, CanonicalName |
Sort-Object CanonicalName |
Format-Table -AutoSize

# Verificar OUs específicas creadas
Get-ADOrganizationalUnit -Filter 'Name -like "*"' -SearchBase "OU=Empresa,DC=idat,DC=pe" |
Select-Object Name, DistinguishedName, WhenCreated
```
### 2. Verificación de Usuarios
```PowerShell
# Listar todos los usuarios con detalles básicos
Get-ADUser -Filter * -Properties Enabled, LastLogonDate, PasswordLastSet | 
Select-Object Name, SamAccountName, UserPrincipalName, Enabled, 
              @{Name="OU";Expression={$_.DistinguishedName -replace '^.+?,(OU|CN)='}},
              LastLogonDate, PasswordLastSet |
Sort-Object Name |
Format-Table -AutoSize

# Listar usuarios por OU específica (ej: Ventas)
Get-ADUser -Filter * -SearchBase "OU=Ventas,OU=Empresa,DC=idat,DC=pe" -Properties * |
Select-Object Name, SamAccountName, EmailAddress, Title, Department |
Format-Table -AutoSize
```

### 3. Verificación de Grupos
```PowerShell
# Listar todos los grupos creados
Get-ADGroup -Filter * -SearchBase "OU=Empresa,DC=idat,DC=pe" -Properties * |
Select-Object Name, Description, DistinguishedName, GroupCategory, GroupScope |
Sort-Object Name |
Format-Table -AutoSize

# Verificar miembros de grupos específicos
$grupos = @("Ventas_Managers", "TI_Admins", "Finanzas_Directores")
foreach ($grupo in $grupos) {
    Write-Host "`nMiembros del grupo $grupo :" -ForegroundColor Cyan
    Get-ADGroupMember -Identity $grupo | 
    Select-Object Name, SamAccountName, DistinguishedName |
    Format-Table -AutoSize
}
```
### 4. Verificación Completa de Asignaciones
```PowerShell
# Generar reporte completo de usuarios y sus grupos
$report = foreach ($user in (Get-ADUser -Filter * -SearchBase "OU=Empresa,DC=idat,DC=pe")) {
    $groups = Get-ADPrincipalGroupMembership -Identity $user | 
              Where-Object { $_.Name -notlike "Domain Users" } | 
              Select-Object -ExpandProperty Name
    
    [PSCustomObject]@{
        Usuario      = $user.Name
        Login        = $user.SamAccountName
        OU           = ($user.DistinguishedName -split ",OU=")[1]
        Grupos       = ($groups -join ", ")
        UltimoLogin  = $user.LastLogonDate
        CuentaHabilitada = $user.Enabled
    }
}

# Exportar reporte a CSV
$report | Sort-Object OU, Usuario | Export-Csv -Path "AD_Users_Report.csv" -NoTypeInformation -Encoding UTF8

# Mostrar resumen en pantalla
$report | Sort-Object OU, Usuario | Format-Table -AutoSize
```
## Recomendaciones para la Verificación

### 1. Para validar usuarios:
```PowerShell
# Verificar usuarios deshabilitados
Get-ADUser -Filter 'Enabled -eq $false' -SearchBase "OU=Empresa,DC=idat,DC=pe" | 
Select-Object Name, SamAccountName, DistinguishedName
```
### 2. Para verificar contraseñas:
```PowerShell
# Usuarios con contraseñas que no expiran
Get-ADUser -Filter 'PasswordNeverExpires -eq $true' -SearchBase "OU=Empresa,DC=idat,DC=pe" | 
Select-Object Name, SamAccountName
```
### 3. Para auditoría de grupos:
```PowerShell
# Grupos sin miembros
$groups = Get-ADGroup -Filter * -SearchBase "OU=Empresa,DC=idat,DC=pe"
foreach ($group in $groups) {
    $count = (Get-ADGroupMember -Identity $group).Count
    if ($count -eq 0) {
        [PSCustomObject]@{Group=$group.Name; MemberCount=$count}
    }
}
```
