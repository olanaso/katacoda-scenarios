Ya tenemos `ConfigDefaults.java` y `DefaultSystemClock.java` configurados correctamente. Ahora es momento de crear una factura electrónica.

La creación de una factura es explicada en la documentación oficial [crea-el-xm](https://project-openubl.github.io/docs/xbuilder/create-xml#crea-el-xml).

## Crea `Main.java`

Aquí tienes una clase llamada `Main.java` con en contenido requerido para crear una factura electrónica.

Has click en el siguiente comando para que pueda ser ejecutado:

````shell
cat << EOF > xbuilder-quickstart/src/main/java/org/openubl/xbuilder/Main.java
package org.openubl.xbuilder;

import io.github.project.openubl.xmlbuilderlib.clock.SystemClock;
import io.github.project.openubl.xmlbuilderlib.config.Config;
import io.github.project.openubl.xmlbuilderlib.facade.DocumentManager;
import io.github.project.openubl.xmlbuilderlib.facade.DocumentWrapper;
import io.github.project.openubl.xmlbuilderlib.models.catalogs.Catalog6;
import io.github.project.openubl.xmlbuilderlib.models.input.common.ClienteInputModel;
import io.github.project.openubl.xmlbuilderlib.models.input.common.ProveedorInputModel;
import io.github.project.openubl.xmlbuilderlib.models.input.standard.DocumentLineInputModel;
import io.github.project.openubl.xmlbuilderlib.models.input.standard.invoice.InvoiceInputModel;
import io.github.project.openubl.xmlbuilderlib.models.output.standard.invoice.InvoiceOutputModel;

import java.math.BigDecimal;
import java.util.Arrays;

public class Main {

    public static void main(String[] args) throws Exception {
        // General config
        Config config = new ConfigDefaults();
        SystemClock clock = new DefaultSystemClock();

        //Usando el archivo .pfx
        InputStream ksInputStream = new FileInputStream(new File("myCertificate.pfx"));
        CertificateDetails certificate = CertificateDetailsFactory.create(ksInputStream, "myCertificatePassword");

        X509Certificate certificate = certificate.getX509Certificate();
        PrivateKey privateKey = certificate.getPrivateKey();

        // Invoice generation
        InvoiceInputModel input = invoiceFactory();
        DocumentWrapper<InvoiceOutputModel> result = DocumentManager.createXML(input, config, clock);

        // El Sitrng del resultado de la implementacion de XML 
        String xml = "<?xml version="1.0" encoding="ISO-8859-1"?> <Invoice xmlns="urn:oasis:names:specification:ubl:schema:xsd:Invoice-2" xmlns:cac="urn:oasis:names:specification:ubl:schema:xsd:CommonAggregateComponents-2" xmlns:cbc="urn:oasis:names:specification:ubl:schema:xsd:CommonBasicComponents-2" xmlns:ccts="urn:un:unece:uncefact:documentation:2" xmlns:cec="urn:oasis:names:specification:ubl:schema:xsd:CommonExtensionComponents-2" xmlns:ds="http://www.w3.org/2000/09/xmldsig#" xmlns:ext="urn:oasis:names:specification:ubl:schema:xsd:CommonExtensionComponents-2" xmlns:qdt="urn:oasis:names:specification:ubl:schema:xsd:QualifiedDatatypes-2" xmlns:sac="urn:sunat:names:specification:ubl:peru:schema:xsd:SunatAggregateComponents-1" xmlns:udt="urn:un:unece:uncefact:data:specification:UnqualifiedDataTypesSchemaModule:2" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" > <ext:UBLExtensions> <ext:UBLExtension> <ext:ExtensionContent/> </ext:UBLExtension> </ext:UBLExtensions> <cbc:UBLVersionID>2.1</cbc:UBLVersionID> <cbc:CustomizationID>2.0</cbc:CustomizationID> <cbc:ID>F001-1</cbc:ID> <cbc:IssueDate>2021-09-12</cbc:IssueDate> <cbc:IssueTime>17:48:25</cbc:IssueTime> <cbc:InvoiceTypeCode listID="0101" listAgencyName="PE:SUNAT" listName="Tipo de Documento" listURI="urn:pe:gob:sunat:cpe:see:gem:catalogos:catalogo01">01</cbc:InvoiceTypeCode> <cbc:DocumentCurrencyCode listID="ISO 4217 Alpha" listAgencyName="United Nations Economic Commission for Europe" listName="Currency">PEN</cbc:DocumentCurrencyCode> <cac:Signature> <cbc:ID>12345678912</cbc:ID> <cac:SignatoryParty> <cac:PartyIdentification> <cbc:ID>12345678912</cbc:ID> </cac:PartyIdentification> <cac:PartyName> <cbc:Name><![CDATA[Los grandes S.A.C.]]></cbc:Name> </cac:PartyName> </cac:SignatoryParty> <cac:DigitalSignatureAttachment> <cac:ExternalReference> <cbc:URI>#PROJECT-OPENUBL-SIGN</cbc:URI> </cac:ExternalReference> </cac:DigitalSignatureAttachment> </cac:Signature> <cac:AccountingSupplierParty> <cac:Party> <cac:PartyIdentification> <cbc:ID schemeID="6" schemeAgencyName="PE:SUNAT" schemeName="Documento de Identidad" schemeURI="urn:pe:gob:sunat:cpe:see:gem:catalogos:catalogo06">12345678912</cbc:ID> </cac:PartyIdentification> <cac:PartyLegalEntity> <cbc:RegistrationName><![CDATA[Los grandes S.A.C.]]></cbc:RegistrationName> <cac:RegistrationAddress> <cbc:AddressTypeCode>0000</cbc:AddressTypeCode> </cac:RegistrationAddress> </cac:PartyLegalEntity> </cac:Party> </cac:AccountingSupplierParty> <cac:AccountingCustomerParty> <cac:Party> <cac:PartyIdentification> <cbc:ID schemeID="6" schemeAgencyName="PE:SUNAT" schemeName="Documento de Identidad" schemeURI="urn:pe:gob:sunat:cpe:see:gem:catalogos:catalogo06">12121212121</cbc:ID> </cac:PartyIdentification> <cac:PartyLegalEntity> <cbc:RegistrationName><![CDATA[Pepito Suarez]]></cbc:RegistrationName> </cac:PartyLegalEntity> </cac:Party> </cac:AccountingCustomerParty> <cac:PaymentTerms> <cbc:ID>FormaPago</cbc:ID> <cbc:PaymentMeansID>Contado</cbc:PaymentMeansID> <cbc:Amount currencyID="PEN">2360</cbc:Amount> </cac:PaymentTerms> <cac:TaxTotal> <cbc:TaxAmount currencyID="PEN">360</cbc:TaxAmount> <cac:TaxSubtotal> <cbc:TaxableAmount currencyID="PEN">2000</cbc:TaxableAmount> <cbc:TaxAmount currencyID="PEN">360</cbc:TaxAmount> <cac:TaxCategory> <cbc:ID schemeAgencyName="United Nations Economic Commission for Europe" schemeID="UN/ECE 5305" schemeName="Tax Category Identifie">S</cbc:ID> <cac:TaxScheme> <cbc:ID schemeAgencyName="PE:SUNAT" schemeID="UN/ECE 5153" schemeName="Codigo de tributos">1000</cbc:ID> <cbc:Name>IGV</cbc:Name> <cbc:TaxTypeCode>VAT</cbc:TaxTypeCode> </cac:TaxScheme> </cac:TaxCategory> </cac:TaxSubtotal> </cac:TaxTotal> <cac:LegalMonetaryTotal> <cbc:LineExtensionAmount currencyID="PEN">2000</cbc:LineExtensionAmount> <cbc:TaxInclusiveAmount currencyID="PEN">2360</cbc:TaxInclusiveAmount> <cbc:PayableAmount currencyID="PEN">2360</cbc:PayableAmount> </cac:LegalMonetaryTotal> <cac:InvoiceLine> <cbc:ID>1</cbc:ID> <cbc:InvoicedQuantity unitCode="NIU" unitCodeListAgencyName="United Nations Economic Commission for Europe" unitCodeListID="UN/ECE rec 20">10</cbc:InvoicedQuantity> <cbc:LineExtensionAmount currencyID="PEN">1000</cbc:LineExtensionAmount> <cac:PricingReference> <cac:AlternativeConditionPrice> <cbc:PriceAmount currencyID="PEN">118</cbc:PriceAmount> <cbc:PriceTypeCode listAgencyName="PE:SUNAT" listName="Tipo de Precio" listURI="urn:pe:gob:sunat:cpe:see:gem:catalogos:catalogo16">01</cbc:PriceTypeCode> </cac:AlternativeConditionPrice> </cac:PricingReference> <cac:TaxTotal> <cbc:TaxAmount currencyID="PEN">180</cbc:TaxAmount> <cac:TaxSubtotal> <cbc:TaxableAmount currencyID="PEN">1000</cbc:TaxableAmount> <cbc:TaxAmount currencyID="PEN">180</cbc:TaxAmount> <cac:TaxCategory> <cbc:ID schemeAgencyName="United Nations Economic Commission for Europe" schemeID="UN/ECE 5305" schemeName="Tax Category Identifier">S</cbc:ID> <cbc:Percent>18</cbc:Percent> <cbc:TaxExemptionReasonCode listAgencyName="PE:SUNAT" listName="Afectacion del IGV" listURI="urn:pe:gob:sunat:cpe:see:gem:catalogos:catalogo07">10</cbc:TaxExemptionReasonCode> <cac:TaxScheme> <cbc:ID schemeAgencyName="PE:SUNAT" schemeID="UN/ECE 5153" schemeName="Codigo de tributos">1000</cbc:ID> <cbc:Name>IGV</cbc:Name> <cbc:TaxTypeCode>VAT</cbc:TaxTypeCode> </cac:TaxScheme> </cac:TaxCategory> </cac:TaxSubtotal> </cac:TaxTotal> <cac:Item> <cbc:Description><![CDATA[Item1]]></cbc:Description> </cac:Item> <cac:Price> <cbc:PriceAmount currencyID="PEN">100</cbc:PriceAmount> </cac:Price> </cac:InvoiceLine> <cac:InvoiceLine> <cbc:ID>2</cbc:ID> <cbc:InvoicedQuantity unitCode="NIU" unitCodeListAgencyName="United Nations Economic Commission for Europe" unitCodeListID="UN/ECE rec 20">10</cbc:InvoicedQuantity> <cbc:LineExtensionAmount currencyID="PEN">1000</cbc:LineExtensionAmount> <cac:PricingReference> <cac:AlternativeConditionPrice> <cbc:PriceAmount currencyID="PEN">118</cbc:PriceAmount> <cbc:PriceTypeCode listAgencyName="PE:SUNAT" listName="Tipo de Precio" listURI="urn:pe:gob:sunat:cpe:see:gem:catalogos:catalogo16">01</cbc:PriceTypeCode> </cac:AlternativeConditionPrice> </cac:PricingReference> <cac:TaxTotal> <cbc:TaxAmount currencyID="PEN">180</cbc:TaxAmount> <cac:TaxSubtotal> <cbc:TaxableAmount currencyID="PEN">1000</cbc:TaxableAmount> <cbc:TaxAmount currencyID="PEN">180</cbc:TaxAmount> <cac:TaxCategory> <cbc:ID schemeAgencyName="United Nations Economic Commission for Europe" schemeID="UN/ECE 5305" schemeName="Tax Category Identifier">S</cbc:ID> <cbc:Percent>18</cbc:Percent> <cbc:TaxExemptionReasonCode listAgencyName="PE:SUNAT" listName="Afectacion del IGV" listURI="urn:pe:gob:sunat:cpe:see:gem:catalogos:catalogo07">10</cbc:TaxExemptionReasonCode> <cac:TaxScheme> <cbc:ID schemeAgencyName="PE:SUNAT" schemeID="UN/ECE 5153" schemeName="Codigo de tributos">1000</cbc:ID> <cbc:Name>IGV</cbc:Name> <cbc:TaxTypeCode>VAT</cbc:TaxTypeCode> </cac:TaxScheme> </cac:TaxCategory> </cac:TaxSubtotal> </cac:TaxTotal> <cac:Item> <cbc:Description><![CDATA[Item2]]></cbc:Description> </cac:Item> <cac:Price> <cbc:PriceAmount currencyID="PEN">100</cbc:PriceAmount> </cac:Price> </cac:InvoiceLine> </Invoice>";


        String signatureID = "mySignID"; // Your Signature ID

        // Get your certificate using the method of your preference
        X509Certificate certificate;
        PrivateKey privateKey;
        // Firmando el XML del comprobante electronico con XML Singer
        Document signedXML = XMLSigner.signXML(xml, signatureID, certificate, privateKey);
        
    }

    

}
EOF
```{{execute}}

## Observa el contenido de `Main.java`

Abre el archivo `xbuilder-quickstart/src/main/java/org/openubl/xbuilder/Main.java`{{open}} y analiza el contenido.