# Laravel

## 1º Instalação do composer
baixar o composer e instalar para verificar usar o comando 'composer'.
<br>
## 2º Instalação do Laravel e projeto em pasta especifica
composer create-project --prefer-dist laravel/laravel name-project'
<br>
## 3º Cria o banco de dados sem as tabelas.
<br>

## 4º Configurações do banco mysql
env.

##5º Gerar migrates
php artisan migrate (caso as tabelas ja tenha sido criadas)
devem começar com create e terminar com table (Exemplo:create_password_resets_table)
'php artisan make:migration create_products_table' OU
opção menos -m indica que é para criar o Model e também o arquivo de migration
'php artisan make:model Product –m'
<br>

##6º Definir dados no migrates
Schema::create('nome_tabela_aqui', function (Blueprint $table) { 
	$table->increments('id');// Id da tabela (chave primária e incremento)
    $table->integer('category_id')->unsigned();// Id da tabela categories
    $table->foreign('category_id')->references('id')->on('categories'); // Define o campo como chave extrangeira (foreign key)
    $table->string('name', 100)->unique(); // Nome do Produto
    $table->string('image', 100)->nullable(); // Imagem do Produto
    $table->text('description')->nullable(); // Descrição do produto
    $table->timestamps();
});
<br>

##7º Gerar atualizaçoes da tabela
gerar (criar) as tabelas : php artisan migrate
para deletar : php artisan migrate:rollback ou php artisan migrate:reset
para atualizar: php artisan migrate:refresh
<br>
##8º Controller
Para criar um controller e seu CRUD
'php artisan make:controller CategoryController --resource'

##9º Rotas
Route::resource('url','NomeController@nomeMetodo');->funciona se o controller feito com resource
Route::get('url','NomeController@nomeMetodo');->retornar algo, como por exemplo uma listagem
Route::post('url','NomeController@nomeMetodo');->cadastrar algo no sistema
Route::put('url','NomeController@nomeMetodo');->editar algum registro.
Route::delete('url','NomeController@nomeMetodo');->para deletar algo.
Route::any('url','NomeController@nomeMetodo');->aceita qualquer requisição, não importa se é GET, POST, PUT, PATH ou DELETE

###Rotas Nomeadas
Route::get('contact', 'SiteControler@contact')->name('contact');
Route::get('post/{id}', 'PostControler@show')->name('post.show');

###Grupo de Rotas:
//admin
Route::group(['prefix' => 'admin'], function(){
    Route::get('users', 'UserController@index');
    Route::get('financeiro', 'FinanceiroController@index');
    Route::get('/', 'AdminController@index');
    //Equivale ao acessar: http://sua-app.dev/admin
});

###Middleware:
permitir acesso a apenas usuários autenticados
Route::get('financeiro','FinanceiroController@index')->middleware('auth');
<br>

## Login sem admiinlte

composer require laravel/ui --dev (NO LARAVEL 6 SUBSTITUI O ARTISAN MAKE:AUTH)
php artisan ui vue --auth
php artisan migrate

## Gerar chave 

php artisan key:generate

## gerar crud no infyom

php artisan infyom:scaffold aluno --fromTable --tableName=aluno

## para dar o refrech nas tabelas 

$table->softDeletes();

## CONFIGURAR EMAIL GMAIL
   config/mail.php
   'host' => env('MAIL_HOST', 'smtp.gmail.com'),
   'from' => [
        'address' => env('MAIL_FROM_ADDRESS', 'camuelsantos@gmail.com'),
        'name' => env('MAIL_FROM_NAME', 'vegefoods'),
    ],

    rota: 
    Route::post('/envio','EmailController@store')->name('mail.store');

    controle:

    use Illuminate\Support\Facades\Mail;
    use Illuminate\Support\Facades\Redirect;

    class EmailController extends Controller
{
    public function store(Request $request)
    {

        Mail::send('contatoEmail', $request->all(), function ($msg) {
            $msg->subject('Vegefoods Pedido');
            $msg->to('camuelsantos@gmail.com');
        });
       return Redirect::to('/');

    }
}

env:

MAIL_DRIVER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=camuelsantos@gmail.com
MAIL_PASSWORD=etujeeaypglrkwqb
MAIL_ENCRYPTION=tls

form so funciona com esse :{!! Form:: open(['route'=>'mail.store', 'method' => 'POST']) !!}{!!Form:: close()!!}

## Chamando um arquivo JS no laravel
   <script type="text/javascript" src="<?php echo asset('public/js/funcao.js')?>"></script>
## Comando para criar Model/Controller/Rotas ao mesmo tempo
   php artisan make:model Product --mcr
## Comando para dropar dados e criar novamente as migrate
   php artisan migrate:fresh
