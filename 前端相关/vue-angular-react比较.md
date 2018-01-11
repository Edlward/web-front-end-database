
## 属性绑定和数据绑定

- angular
		
		<p>
		    <app-stars [rating]="product?.rating"></app-stars>
		</p>
		和
	    <button class="btn btn-default btn-lg"
	        [class.active]="isWatched"
	        (click)="watchProduct()"
	    >
	      {{ isWatched ? '取消关注' : '关注'}}
	    
		</button>

- vue

		<check-order :is-show-check-dialog="isShowCheckOrder"
                     :order-id="orderId"
                     @on-close-check-dialog="hideCheckOrder">

        </check-order>
		和
        <router-link v-for="(item, index) in products"
            :to="{ path: item.path }"
            tag="li" active-class="active"
            :key="index"
        >
            {{ item.name }}
        </router-link>

- react

		<div>
            <input type="text" value={inputVal} 
				placeholder="input data" 
				onChange={this.handleChange} 
			/> 
            <h4>{inputVal}</h4>
         </div>;











