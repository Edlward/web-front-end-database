
## *ngFor
	
	这个指令需要在 app.module.ts 中引入

	import { CommonModule } from '@angular/common';
	并加入到
	    imports: [
        CommonModule,
        RouterModule.forChild(homeRoutes)
    ],
	
	如果这个指令在 子组件中使用，而且 子组件是通过动态路由加载的话，子组件的 xx.module.ts 文件中也要加入
		import { CommonModule } from '@angular/common';
		并加入到
		    imports: [
	        CommonModule,
	        RouterModule.forChild(homeRoutes)
	    ],


## ngClass 相关用法 

        <div
            *ngFor="let item of boardList; let i=index"
            class="product-item"
            [class.item]="i % 2 !== 0"
            [class.removeBottom]="i >= (boardList.length -2)"
            [ngClass]="classArr[i]"
        >
            <div class="content-padding">
                <div class="product-item-inner" >
                    <h2>{{ item.title }}</h2>
                    <p>{{ item.description }}</p>
                    <div class="buy-product-button">
                        <a [routerLink]="['detail/' + item.toKey]">
                            立即购买
                        </a>
                    </div>
                </div>
            </div>
        </div>


