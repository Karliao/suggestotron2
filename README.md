@topics = Topic.joins("left join (select topic_id,count(*) cnt from votes group by topic_id) t1 on t1.topic_id=topics.id").order("t1.cnt desc").all

def upvote
  @topic = Topic.find(params[:id])
  @topic.votes.create
  redirect_to (topics_path)
end

def downvote
  @topic = Topic.find(params[:id])
  @topic.votes.last.destroy
  redirect_to (topics_path)
end

<tbody>
  <% @topics.each do |topic| %>
    <tr>
      <td><%= link_to topic.title, topic %></td>
      <td><%=  pluralize(topic.votes.count, "vote")  %></td>
      <td><%= button_to '+1', upvote_topic_path(topic), method: :post %></td>
      <td><%= button_to '-1', downvote_topic_path(topic), method: :post %></td>
      <td><%= link_to 'Show', topic %></td>
      <td><%= link_to 'Edit', edit_topic_path(topic) %></td>
      <td><%= link_to 'Destroy', topic, method: :delete, data: { confirm: 'Are you sure?' } %></td>
    </tr>
  <% end %>
</tbody>

<%= link_to 'New Topic', new_topic_path %>
<a href="/about.html">about</a>
