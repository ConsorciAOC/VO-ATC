# VO-ATC

## INDEX

- [1. Introducció](#1)
- [2. Transmissions de dades disponibles](#2)
- [3. Missatgeria del servei](#3)
   * [3.1 Situació de deute d’un contribuent (ATC_INF_DEUTES_TMP)](#3.1)
        * [3.1.1 Petició – dades genèriques](#3.1.1)
		* [3.1.2 Petició – dades específiques](#3.1.2)
		    *  [3.1.2.1 Dades d’error](#3.1.2.1)
- [4. Joc de proves](#4)

# Introducció <a name="1"></a>

Aquest document detalla la missatgeria associada al servei de l’Agència Tributària de Catalunya (en endavant ATC).

Per poder realitzar la integració cal conèixer prèviament la següent documentació:
*	Document del Servei Via Oberta.
- [Document de Missatgeria Genèrica de la PCI del Consorci AOC.][PCI]
 
[PCI]:https://github.com/ConsorciAOC/PCI

# 2	Transmissions de dades disponibles <a name="2"></a>

Les dades disponibles a través del servei són les que es presenten a continuació:

Emissor: ATC (Agència Tributària de Catalunya)

| **PRODUCTE** | **MODALITAT** | **DESCRIPCIO** |
|---|---|---|
| ATC | ATC\_INF\_DEUTES\_TMP | Consultadesituaciódedeuted&#39;uncontribuenta Catalunya. |

# 3	Missatgeria del servei <a name="3"></a>

A continuació es detalla la missatgeria corresponent a les modalitats de consum del producte ATC.

## 3.1	Situació de deute d’un contribuent (ATC_INF_DEUTES_TMP) <a name="3.1"></a>

A partir de les dades d’una persona física (NIF o NIE) o una persona jurídica (CIF) informa si té o no deutes.

Les possibles respostes són:
•	Persona localitzada amb deute
•	Persona localitzada sense deute
•	Persona no localitzada
•	Localitzades diverses persones amb les dades proporcionades

En cas de que el titular es localitzi, en la resposta s’inclou un document (PDF o WORD) amb el certificat de l’estat del deute.

### 3.1.1	Petició – dades genèriques <a name="3.1.1"></a>

Al bloc de dades genèriques de les peticions caldrà informar les dades del titular del qual es vol consultar la situació de deute.

| Element | Descripció |
| --- | --- |
| Peticion/Solicitudes/SolicitudTransmision/atosGenericos/Titular | Bloc de dades on s’informa el titular del qual es vol consultar l’estat del deute. |
| //Titular/TipoDocumentacion | Obligatori. Tipus de documentació del titular: NIF, NIE o CIF. |
| //Titular/Documentacion | Obligatori. Documentació del titular. |
| //Titular/Nombre | Obligatori. Nom del titular si és persona física o raó social si és jurídica. |
| //Titular/Apellido1 | Obligatori. Primer cognom del titular si és persona física. |
| //Titular/Apellido2 | Obligatori. Segon cognom del titular si és persona física. |
| //Titular/NombreCompleto | Opcional. Si no s’informa es composa automàticament a partir dels valors dels elements Nombre, _Apellido1_ i _Apellido2_. |

Addicionalment, l’emisor final requereix que s’informin les dades del funcionari que realitza la consulta.

| Element | Descripció |
| --- | --- |
| Peticion/Atributos/Funcionario/NombreCompletoFuncionario | Nom del funcionari. |
| Peticion/Atributos/Funcionario/NifFuncionario | NIF del funcionari. |
| Peticion/Solicitudes/SolicitudTransmision/DatosGenericos/Solicitante/Funcionario/NombreCompletoFuncionario | Nom del funcionari. |
| Peticion/Solicitudes/SolicitudTransmision/DatosGenericos/Solicitante/Funcionario/NifFuncionario | NIF del funcionari. |

No es necessari informar el bloc de dades específiques.

### 3.1.2	Resposta – dades específiques <a name="3.1.2"></a>

A continuació es descriu el missatge corresponent al bloc de dades específiques generat per l’emisor final de les dades.

| Element | Descripció |
| --- | --- |
| InfDeutePICAResponse/Informacio | Bloc de dades específiques corresponents a la resposta. |
| //Informacio/Csv | Codi Segur de Verificació. La validesa del certificat es pot comprovar a la web [www.e-tributs.cat.](http://www.e-tributs.cat/) |
| //Informacio/DataCaducitat | Data de validesa de la consulta. L’emisor final no informa aquest element ja que la resposta és vàlida únicament en l’instant en que es realitza la consulta. |
| //Informacio/Document | Certificat de situació de deute codificat en base 64. |
| //Informacio/FormatDocument | Format del certificat de deute (PDF o WORD). |
| //Informacio/ImportTotal | Import del deute registrat. |
| //Informacio/Resposta | Codi de resultat de la resposta: Persona localitzada amb deute, Persona localitzada sense deute, Persona no localitzada i Localitzades diverses persones amb les dades proporcionades |
| InfDeutePICAResponse/InfDeutePICARequest | Bloc de dades de la resposta. |
| ///InfDeutePICARequest/Idioma | Català. |
| ///InfDeutePICARequest/Nom | Nom del titular informat a la petició. |
| ///InfDeutePICARequest/NomComplet | Nom complert del titular informat a la petició. |
| ///InfDeutePICARequest/Cognom1 | Primer cognom del titular informat a la petició. |
| ///InfDeutePICARequest/Cognom2 |Segon cognom del titular informat a la petició. |
| ///InfDeutePICARequest/NumeroDocument | Documentació del titular informat a la petició. |
| ///InfDeutePICARequest/TipusDocumentacio | Tipus de documentació del titular informat a la petició. |
| InfDeutePICAResponse/PICAError | Detalls de l’incidència en cas d’error processant la petició. Vegeu apartat 3.1.2.1. |

#### 3.1.2.1	Dades d’error <a name="3.1.2.1"></a>

![Dades d'error](https://github.com/ConsorciAOC/VO-ATC/blob/main/images/3%201%202%201%20Dades%20d%E2%80%99error.png)

| Element | Descripció |
| --- | --- |
| //PICAError/CodiError | Codi d’error informat per l’emissor. |
| //PICAError/Error | Literal o excepció associada a l'error. |
| //PICAError/Descripcio | Descripció de l'error. |
| //PICAError/Causa | Traça de detall de l'error. |

# 4 Joc de proves <a name="4"></a>

| Tipus doc. | Documentació | Nom | Primer cognom | Segon cognom | Situació dedeute |
| --- | --- | --- | --- | --- | --- |
| NIF | 35073919T | CRISTINA | SIALO | SIAHE | Sensedeute |
| CIF | B63422257 | XELTONTRADES.L. ||| Sensedeute |
| NIF | 37171951H | ROSAM | ROURA | CARLES | Ambdeute |
| CIF | A46201992 | MUSICALOESTESA ||| Ambdeute |
| CIF | B60304979 | FERRANDAVIDSL ||| Ambdeute |
| CIF | 37824983B | JOSELUIS | MARCO | ABAD | Sensedeute |
| NIF | 17141391C | MARIA DELC | MARCO | ABADIA | Sensedeute |
| NIF | 37227164P | JOAQUINA | MARCO | ABELLA | Sensedeute |

L'emissor final publica els següent [joc de proves a l'entorn de pre-producció][proves]
 
[proves]: http://transversals.ctti.intranet.gencat.cat/sol-pica-iop-gene/
 
![image](https://user-images.githubusercontent.com/32306731/137281698-9dfc2044-94f7-487f-a7d6-9a4e0707feb3.png) En cas de tindre problemes per accedir als jocs de proves, si us plau, obre un tiquet a través del [formulari][form]
 
[form]:https://suport.aoc.cat/hc/ca/requests/new
