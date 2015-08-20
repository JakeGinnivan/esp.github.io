Getting Started
---------------

Sample code below to test things out.

.. example-code::
	.. code-block:: js

		// Create an event raiser and publish an event
		class CarScreenController {
			constructor(router) {
				this._router = router;
			}
			start() {
				this._listenForModelChanges();

				console.log("Simulating some user actions over 4 seconds: ");
				setTimeout(() => {
					this._router.publishEvent('myModelId', 'carMakeChangedEvent', { make: 'BMW' });
				}, 0);
				setTimeout(() => {
					this._router.publishEvent('myModelId', 'isSportModelChangedEvent', { isSportModel: true });
				}, 2000);
				setTimeout(() => {
					this._router.publishEvent('myModelId', 'colorChangedEvent', { color: 'blue' });
				}, 2000);
			}
			_listenForModelChanges() {
				this._router
					.getModelObservable('myModelId')
					.observe(model => {
						// you'd sync your view here, for now just dump the description to the console
						console.log(model.description);
					});
			}
		}

	.. code-block:: csharp

		[Test]
		public void RemovalByAPreProcessorEndsEventWorkflow()
		{
			_model1PreEventProcessor.RegisterAction(model =>
			{
				_router.RemoveModel(_model1.Id);
			});
			_router.PublishEvent(_model1.Id, new Event1());
			_model1EventProcessor.Event1Details.PreviewStage.ReceivedEvents.Count.ShouldBe(0);
			_model1EventProcessor.Event1Details.NormalStage.ReceivedEvents.Count.ShouldBe(0);
			_model1EventProcessor.Event1Details.CommittedStage.ReceivedEvents.Count.ShouldBe(0);
			_model1Controller.ReceivedModels.Count.ShouldBe(0);
		}
