---
layout: post
title: Ajax로 Servelt과 데이터 주고받기
category: Ajax
tags: [Ajax, Servlet, JSP, JQuery, JSON]
---

### 알아두어야 할 것
1. 오류가 났을 때는 console.log로 찍어서 확인해보기
	> console.log('error');
	
    개발자도구 콘솔에서 확인 가능

2. 이름을 지을 때는 꼭 단수, 복수, list등에 유의해서 짓기 - 나중에 헷갈리지 않음

3. HTML5에서는 data라고 임의 지정 값이 있음
	> <div data-animal-type="bird">Owl</div> 
	> $(this).data('animalType')
  
   불러올땐 이렇게 Camel Case로 함 
	
    - [data-* Attributes](https://www.w3schools.com/tags/att_global_data.asp)

4. DAO에서 get으로 불러올때는 id로 불러오는 게 좋다 
	- id는 primary key이기 때문에 중복이 없다. 

5. <script>의 위치는 
-  html파일의 로딩이 끝난 후인 </body> 앞쪽이 좋다. 
   	이게 호출되는 시점에 버튼에 이벤트가 바인딩(붙는)되는 것이기 때문에,  호출되는 시점을 문서가 로드된 이 후에 되도록 해야한다.

-  만약 위에 두려면 이렇게 하는게 좋다.
	> 
  
    
    
        $(document).ready(function() {
        console.log('document loaded');
        });
	
  여기서 하려면 요렇게 도큐먼트가 레디가 되었을 때 이벤트를 설정해줘야 한다.

	
—


### Servlet
>  




  
    
    String id = request.getParameter("id");
		...
	response.setContentType("application/json");
	response.getWriter().append("{ \"updatedType\": \"")
			.append(updatedType)
			.append("\"}");
	response.getWriter().flush();
	response.getWriter().close();

임의로 JSON과 같은 형식으로 만듦


### JSP(Ajax 부분만)
> 	




    $('.btn').click(function() {
		// <input ... data-todo-id="THIS!!">
		// $(this).attr('data-todo-id'); 이렇게 받아올 수 있음 
		// console.log($(this).data('todoId'), $('#todotitle').text());
		var $btn = $(this);
		
		$.ajax({
			url : '/Todo/TodoTypeServlet',
			type : 'post',
			data : {
				id : $(this).data('todoId')
			},
			accept: 'application/json',
			success: function(response) {
				console.log('next status.', response);
				var card = $btn.parent();
				card.detach();
				
				if(response.updatedType === 'DOING') {
					$('#doingList').append(card);
				} else {
					$('#doneList').append(card);
					$btn.detach();
				}
			}
		});
	})

