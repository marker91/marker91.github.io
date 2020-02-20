## Описание моделей

    // Модель ошибки
    
    {
    
        objectType: "ERROR"
    
        title: "<Заголовок>"
        
        message: "<Сообщение>"
    
    } 

---

    // Модель заказа расписания
    
    {
    
        objectType: "SCHEDULE"
    
        visitId: "<UUID идентификатор визита>"
    
        orderId: "<UUID идентификатор заказа>"
        
        contactId: "<UUID идентификатор пациента>"
        
        orderProducts: [
        
            schedule.OrderProduct {
            
                orderProductId: "<UUID идентификатор продукта заказа>"
                
                productId: "<UUID идентификатор продукта>"
            
            }
        
        ]
    
    }

---

    // Модель загрузки заказа расписания
    
    {
    
        action: "LOAD"
        
        payload: null
    
    }

---

    // Модель создания заказа расписания
    
    {
    
        action: "CREATE"
        
        payload: {
   
            contactId: "<UUID идентификатор пациента>"

            productPlannedTimes: [
                         
                {
                             
                    productId: "<UUID идентификатор продукта>"
                                 			
                    plannedTime: <Дата планирвоания продукта, тип Date>
                           
                }
                         
            ]
                     
        }
    
    }

---

    // Модель установки планирования продукта заказа
    
    {
    
        action: "UPDATE_PLANNED_TIMES"
        
        payload: {
                     
            orderProductId: "<UUID идентификатор продукта заказа>"
                         
            plannedTime: <Дата планирвоания продукта, тип Date>
                     
        }
    
    }

---

        // Модель перехода к оформлению заказа в АРМПС2
        
        {
        
            action: "GO_TO_ORDER"
            
            payload: {
                         
                orderId: "<UUID идентификатор заказа>"
                         
            }
        
        }

## Описание взаимодействия

    // Подписка на событие postMessage
    
    window.addEventListener("message", function (event) {
        console.info(event.data);
    }, false);

---

    // Загрузка текущего заказа
    
    let result = { 
        action: "LOAD", 
        payload: null 
    };
    
    let json = JSON.parse(JSON.stringify(result));
    
    parent.postMessage(json, "*");
    
    После загрузки заказа возвращается модель заказа расписания с полем objectType === "SCHEDULE",
    если при загрузке заказа возникнет ошибка, то вернется модель ошибки с полем objectType === "ERROR", 
    так же значение может быть null, если отсутствует идентификатор заказа.

---

    // Создания нового заказа
    
    let orderPlace = {};
    
    orderPlace.contactId = "836D505F-C7B4-11E8-B7C8-5433952BAE0D";
    
    let plannedProduct = {};
    plannedProduct.productId = "DAE1CD37-800B-4B66-82C8-CDACE2014CA5";
    plannedProduct.plannedTime = new Date();
     
    orderPlace.productPlannedTimes = [plannedProduct];
     
    let result = {
        action: "CREATE",
        payload: orderPlace
    };
     
    let json = JSON.parse(JSON.stringify(result));
    
    parent.postMessage(json, "*");
    
    После создания нового заказа возвращается модель заказа расписания с полем objectType === "SCHEDULE",
    если при создании нового заказа возникнет ошибка, то вернется модель ошибки с полем objectType === "ERROR".

---

    // Переход к оформлению заказа в АРМПС2
    
    let result = {
        action: "GO_TO_ORDER",
        payload: {
            orderId: "836D505F-C7B4-11E8-B7C8-5433952BAE0D"
        }
    }
    
    let json = JSON.parse(JSON.stringify(result));
        
    parent.postMessage(json, "*");
    
    Переход к оформлению заказа в АРМПС2 не возвращает значение,
    если передан пустой идентификатор заказа, то вернется модель ошибки с полем objectType === "ERROR".

---

    // Установка даты планирования продуктов заказа
            
    let plannedOrderProduct = {};
    plannedOrderProduct.orderProductId = "F1178777-4265-11EA-E8AD-34F64BA47A0F";
    plannedOrderProduct.plannedTime = new Date();
    
    let plannedOrderProductArray = [plannedOrderProduct];
    
    let result = {
        action: "UPDATE_PLANNED_TIMES",
        payload: plannedOrderProductArray
    };
    
    let json = JSON.parse(JSON.stringify(result));
    
    parent.postMessage(json, "*");
    
    Установка даты планирования продукта не возвращает значение,
    если при установке даты планирования продуктов заказа возникнет ошибка, 
    то вернется модель ошибки с полем objectType === "ERROR".