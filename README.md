Importante
==========
Este projeto ainda está em fase inicial. Nem todas as funcionalidades foram testadas ainda. 
Sempre que uma funcionalidade estiver OK, será adicionada aos exemplos logo abaixo.

PHP SDK para [Braspag](http://www.braspag.com.br) API
=====================================================

## Descrição
SDK para facilitar o uso da API Braspag. 

Este projeto foi criado a partir de um fork do [projeto original](https://github.com/Braspag/BraspagApiPhpSdk), 
porém como as mudanças foram muito significativas, resolvi criar um 
novo projeto. 

## Instalação
Adicione "jotjunior/braspag-php-sdk": "dev-master" no seu composer.json.

## Configuração
Por padrão, o arquivo de configuração da aplicação fica no diretório config, 
na raiz do repositório. 

Para reescrever qualuqer atributo da configuraçõa, basta injetar, como um Array, 
no momento de instanciar o objeto.

Exemplo:
```php
<?php
$service = new ApiService([
    'merchantId' => '00000000-0000-0000-0000-000000000000',
    'merchantKey' => '0000000000000000000000000000000000000000',
    'authenticate' => true
]);

```

## Exemplos

### Venda Simplificada

```php
<?php

use Braspag\ApiService;
use Braspag\Model\Sale;

$sale = [
    'merchantOrderId' => 2016080600,
    'customer' => [
        'name' => 'Comprador de Testes',
    ],
    'payment' => [
        'type' => 'CreditCard',
        'amount' => 100,
        'provider' => 'Simulado',
        'installments' => 1,
        'creditCard' => [
            'cardNumber' => '4532117080573700',
            'holder' => 'Test T S Testando',
            'expirationDate' => '12/2021',
            'securityCode' => '000',
            'brand' => 'Visa'
        ]
    ]
];
$sale = new Sale($sale);

$service = new ApiService();
$result = $service->authorize($sale);
```

### Venda Completa

```php
<?php

use Braspag\ApiService;

$sale = [
    'merchantOrderId' => 2016060900,
    'customer' => [
        'name' => 'Comprador de Testes',
        'identity' => '11225468954',
        'identityType' => 'CPF',
        'email' => 'compradordetestes@braspag.com.br',
        'birthDate' => '1991-01-02',
        'address' => [
            'street' => 'Av. Marechal Câmara',
            'number' => 160,
            'complement' => 'Sala 934',
            'zipCode' => '20020-080',
            'district' => 'Centro',
            'city' => 'Rio de Janeiro',
            'state' => 'RJ',
            'country' => 'BRA',
        ],
        'deliveryAddress' => [
            'street' => 'Av. Marechal Câmara',
            'number' => 160,
            'complement' => 'Sala 934',
            'zipCode' => '20020-080',
            'district' => 'Centro',
            'city' => 'Rio de Janeiro',
            'state' => 'RJ',
            'country' => 'BRA',
        ]
    ],
    'payment' => [
        'type' => 'CreditCard',
        'amount' => 100,
        'currency' => 'BRL',
        'country' => 'BRA',
        'provider' => 'Simulado',
        'serviceTaxAmount' => 0,
        'installments' => 1,
        'interest' => \Braspag\Model\Interest::ByMerchant,
        'capture' => true,
        'authenticate' => false,
        'softDescriptor' => 'tst',
        'creditCard' => [
            'cardNumber' => '4532117080573700',
            'holder' => 'Test T S Testando',
            'expirationDate' => '12/2021',
            'securityCode' => '000',
            'saveCard' => false,
            'brand' => 'Visa',
        ],
        'extraDataCollection' => [
            [
                'name' => 'NomeDoCampo',
                'value' => '1234567'
            ]
        ]

    ]

];

$sale = new Braspag\Model\Sale($sale);

$service = new ApiService();
$result = $service->authorize($sale);
```
