#laravel构架
对于之前的MVC框架，写程序的时候，会导致controller过去肥大，后期不容易维护，所以以下整理了适合于开发项目的构架，有助于代码整洁、清晰。
##Controller过于肥大
1、发送Email，使用异步API
2、使用php写的逻辑
3、显示格式转换
4、依需求显示一些数据
5、依需求显示不同的逻辑
大多数以上需求都会写到Controller
基于这样的问题，构架了以下框架
Model      Eloquent class
Repository 辅助model，处理数据库逻辑，然后注入到service。
Service    辅助controller，处理商业逻辑，然后注入到controller。
Controller 接收HTTP request，调用其他service。
Transform  处理接口显示逻辑

Model、Repository、Service、Transform 都是建在App目录下，

Controller 是注入 Service ， Service是注入 Repository， Repository注入Model

``
<?php
/**
 *
 * User: 梁晓伟  lxw11109@gmail.com
 * Date: 2017-09-06
 * Time: 16:06
 */

namespace App\Transformers;


use App\Models\OrderDetail;
use League\Fractal\TransformerAbstract;

class OrderDetailTransformer extends TransformerAbstract
{
    public function transform(OrderDetail $order)
    {
        return [
            'detailId'              =>$order->detailId,
            'skuId'                 =>$order->skuId,
            'commodityName'         =>$order->commodityName,
            'commodityPrice'        =>$order->commodityPrice,
            'commodityIcon'         =>$order->commodityIcon,
            'descPictures'          =>$order->descPictures,
            'amounts'               =>$order->amounts,
            'commodityId'           =>$order->commodityId,
            'skuDesc'               =>$order->skuDesc,
            'discount'              =>$order->discount,
            'totalPrice'            =>$order->totalPrice,
        ];
    }
}
``


