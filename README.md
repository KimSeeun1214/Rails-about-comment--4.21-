# Rails-about-comment--4.21-
# 1. rails generate model Comment commenter:string body:text movie:references

# 2. rake db:migrate

# 3. app/models/movie.rb
has_many :comments, dependent: :destroy

# 4. config/route.rb.
  resources :movies do
    resources :comments
  end


# 5. rails genrate controller Comments

# 6. rake routes

# 7. app/views/movies/show.html.erb
<p>
  <strong>Title:</strong>
  <%= @movie.title %>
</p>

<p>
  <strong>Information:</strong>
  <%= @movie.information%>
</p>

<h3>Comments</h3>
<% @movie.comments.each do |comment| %>
  <p>
    <strong>Commenter:</strong>
    <%= comment.commenter %>
  </p>

  <p>
    <strong>Comment:</strong>
    <%= comment.body %>  
 </p>
  <p>
     <%= link_to 'Destroy Comment', [comment.movie, comment],
 method: :delete, data:{confirm: 'Are you sure?' } %>
  </p>
<% end %>

<h3>Add a comment:</h3>
<%= form_for([@movie, @movie.comments.build]) do |f| %>
  <p>
    <%= f.label :commenter %><br>
    <%= f.text_field :commenter %>
  </p>
  <p>
    <%= f.label :body %><br>
    <%= f.text_area :body %>  
 </p>
  <p>
    <%= f.submit %>
  </p>
<% end %>

<%= link_to 'Back', movies_path %>
<%= link_to 'Edit', edit_movie_path(@movie) %>


# 8. comments_controller.rb
	def create
		@movie = Movie.find(params[:movie_id])
		@comment = @movie.comments.create(comment_params)
		redirect_to movie_path(@movie)
	end
	
	def destroy
		@movie = Movie.find(params[:movie_id])
		@comment = @movie.comments.find(params[:id])
		@comment.destroy
		redirect_to movie_path(@movie)
	end

	private
		def comment_params
			params.require(:comment).permit(:commenter, :body)
		end

# 9. rails s

# 10. app/assets/stylesheets

body { font-family:serif;
}

h1 {    font-family:"verdana", sans-serif;
        font-size: 24pt;
        font-weight: bold;
        color:#F00 ;
}





