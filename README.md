# combinations
$combination = $attributesCombinations[0]

Hi i have a big problem ,i see other tips on same issue but anyone solve mine. I use ps 1.7.7.5 with php 7.3 i'm not a developer Now everytime i try to create an attribute (on old or new product) return me error "can update the setting" so i launch the debug and return that error was on $combination = $attributesCombinations[0]  i see on sql field,cache....everythings but ... someone can help me? thanks

in src/Adapter/CombinationDataProvider.php (line 126)
     *     * @return array     */    public function completeCombination($attributesCombinations, $product)    {        $combination = $attributesCombinations[0];        $attribute_price_impact = 0;        if ($combination['price'] > 0) {            $attribute_price_impact = 1;        } elseif ($combination['price'] < 0) {
     
     CombinationDataProvider->completeCombination(array(), object(Product))
in src/Adapter/CombinationDataProvider.php (line 107)
        $product = new Product($productId);        $combinations = [];        foreach ($combinationIds as $combinationId) {            $combinations[$combinationId] = $this->completeCombination(                $product->getAttributeCombinationsById(                    $combinationId,                    $languageId                ),                $product            );
        
        CombinationDataProvider->getFormCombinations(array('0'), 1)
in src/PrestaShopBundle/Controller/Admin/CombinationController.php (line 53)
        if ($combinationIds === false || count($combinationIds) == 0) {            return $response;        }        $combinationDataProvider = $this->get('prestashop.adapter.data_provider.combination');        $combinations = $combinationDataProvider->getFormCombinations($combinationIds, (int) $this->getContext()->language->id);        $formFactory = $this->get('form.factory');        foreach ($combinations as $combinationId => $combination) {            $forms[] = $formFactory->createNamed(                "combination_$combinationId",
        
        CombinationController->generateCombinationFormAction(array('0'))
in vendor/symfony/symfony/src/Symfony/Component/HttpKernel/HttpKernel.php (line 151)
        $this->dispatcher->dispatch(KernelEvents::CONTROLLER_ARGUMENTS, $event);        $controller = $event->getController();        $arguments = $event->getArguments();        // call controller        $response = \call_user_func_array($controller, $arguments);        // view        if (!$response instanceof Response) {            $event = new GetResponseForControllerResultEvent($this, $request, $type, $response);            $this->dispatcher->dispatch(KernelEvents::VIEW, $event);
        
        HttpKernel->handleRaw(object(Request), 1)
in vendor/symfony/symfony/src/Symfony/Component/HttpKernel/HttpKernel.php (line 68)
    public function handle(Request $request, $type = HttpKernelInterface::MASTER_REQUEST, $catch = true)    {        $request->headers->set('X-Php-Ob-Level', (string) ob_get_level());        try {            return $this->handleRaw($request, $type);        } catch (\Exception $e) {            if ($e instanceof RequestExceptionInterface) {                $e = new BadRequestHttpException($e->getMessage(), $e);            }            if (false === $catch) {
    
    HttpKernel->handle(object(Request), 1, false)
in vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php (line 200)
        $this->boot();        ++$this->requestStackSize;        $this->resetServices = true;        try {            return $this->getHttpKernel()->handle($request, $type, $catch);        } finally {            --$this->requestStackSize;        }    }
        
        Kernel->handle(object(Request), 1, false)
in admin095b5akmg/index.php (line 82)
$request = Request::createFromGlobals();Request::setTrustedProxies([], Request::HEADER_X_FORWARDED_ALL);try {    require_once __DIR__.'/../autoload.php';    $response = $kernel->handle($request, HttpKernelInterface::MASTER_REQUEST, false);    $response->send();    $kernel->terminate($request, $response);} catch (NotFoundHttpException $exception) {    define('ADMIN_LEGACY_CONTEXT', true);    // correct Apache charset (except if it's too late)
